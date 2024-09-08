# Requêtes SQL et PL SQL

<!-- requête de base : CRUD pour chaque entité -->

<!-- Création des entités -->

<!-- Insertion des données -->

## Attention : ce code PL/SQL concerne un autre projet (merci de votre compréhension):

### Bésoin métier/fonctionnel :
Le client souhaite faire afficher la mention __NON RETOURNABLE__ à la fin de la désignation de certains produits :

```sql
drop table g_produits;

create table g_produits (
    ref_interne integer,
    designation varchar2(100)
);

insert into g_produits values (75, 'Lenonvo key boar');
insert into g_produits values (85, 'HP Pavillon');
insert into g_produits values (100, 'Macintosh M3');
insert into g_produits values (4, 'Dicota sac de voyage');

/******************************************************* Procedure *********************************************************/

CREATE OR REPLACE PROCEDURE adding_text_at_the_end
IS
	CURSOR list_of_product IS SELECT * FROM g_produits WHERE ref_interne in (75, 100);
	text_to_add     varchar2(900) := 'NON RETOURNABLE';
	new_designation varchar2(900);
BEGIN
    FOR one_product IN list_of_product
    LOOP
	new_designation := (one_product.designation || ' - NON RETOURNABLE');
        update g_produits set designation = new_designation where ref_interne = one_product.ref_interne;
    END LOOP;
END;
/

select * from g_produits;
execute adding_text_at_the_end;
select * from g_produits;
```

### Même chose que ci-dessus, mais avec FETCH :

```sql
-- Ajouter la mention « NON RETOURNABLE » à une liste de désignation :
drop table g_produits;

create table g_produits (
    ref_interne integer,
    designation varchar2(100)
);

insert into g_produits values (75, 'Lenonvo key boar');
insert into g_produits values (85, 'HP Pavillon');
insert into g_produits values (100, 'Macintosh M3');
insert into g_produits values (4, 'Dicota sac de voyage');

/******************************************************* Procedure *********************************************************/

CREATE OR REPLACE PROCEDURE adding_text_at_the_end
IS
	CURSOR cur_list_of_product IS SELECT * FROM g_produits WHERE ref_interne in (85, 4);
	text_to_add     varchar2(900) := 'NON RETOURNABLE';
	new_designation varchar2(900);
	r_product cur_list_of_product%ROWTYPE;
BEGIN
    OPEN cur_list_of_product;
	LOOP
        FETCH cur_list_of_product into r_product;
		
		EXIT WHEN cur_list_of_product%NOTFOUND;

		new_designation := (r_product.designation || ' - NON RETOURNABLE');

		update g_produits set designation = new_designation where ref_interne = r_product.ref_interne;

    END LOOP;

	CLOSE cur_list_of_product;
END;
/

select * from g_produits;
execute adding_text_at_the_end;
select * from g_produits;
```

### Bésoin métier/fonctionnel :
Le client souhaite faire intégrer du contenu marketing (code HTML/CSS) à la création d'un produit de marque __Lenovo__ et de marque __grade A__

```sql
drop table g_produits;

create table g_produits (
    ref_interne       integer,
    marque            varchar2(100),
    categorie         varchar2(100),
    contenu_marketing varchar2(1000)
);

select * from g_produits;

-- trigger for g_produits_Lenovo_grade_A_trg

create or replace trigger g_produits_lenovo_grade_A_trg
	before insert 
    on    g_produits
    for each row
declare
    new_marque    g_produits.marque%type    := :NEW.marque;
	new_categorie g_produits.categorie%type := :NEW.categorie;
begin
    if new_marque = 'Lenovo' and new_categorie = 'grade A' then
    	:NEW.contenu_marketing := 'blabla blabla';
	end if;
end;
/
-- end trigger for g_produits_Lenovo_grade_A_trg
    
insert into g_produits values (13, 'Lenovo', 'grade A', null);
insert into g_produits values (12, 'Lenovo', 'grade B', null);
insert into g_produits values (11, 'Samsung', 'grade A', null);
insert into g_produits values (14, 'Lenovo', 'grade A', null);
insert into g_produits values (20, 'Toshiba', 'grade C', null);
insert into g_produits values (21, 'Lenovo', 'grade A', null);
    
select * from g_produits;
```
