# Requêtes SQL et PL SQL

<!-- requête de base : CRUD pour chaque entité -->

<!-- Création des entités -->

<!-- Insertion des données -->

## Attention : ce code PL/SQL concerne un autre projet :

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

Même chose que ci-dessus, mais avec FETCH :

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
