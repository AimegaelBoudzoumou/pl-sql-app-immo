# Spécifications fonctionnelles

Voici les principaux éléments qui interagissent au sein de l'application.

Pour chaque élément, le code SQL et PL/SQL se trouve dans un fichier à part.

## Logement	
-	A un état : en travaux, péparation à la location, prêt à la location, en location, en maintenance après état des lieux
-	Pour être visité puis loué, le logement doit être dans l’état « prêt à la location »
-	Peut faire l’objet d’un ou plusieurs travaux en même
-	Peut être loué à seul client en même temps
-	A des caractéristiques : adresse (numéro de rue/avenue, quartier, ville, pays), plan (salon, nombre de chambre, douche/toilette, cuisine, véranda, autre), visuels
-	A un identifiant unique
-	Date d'entrée dans le parc de logement : cette information ainsi que le logement sont enregistrées dans la table __Dates clés sur logement__ (penser par exemple au Trigger)
-	A un descriptif

## Dates clés sur logement
-	Un logement possède des dates clés. La première date est celle de l'enregistrement du logement dans le parc immobilier. Ensuite, les grands faits marquants (ex. signature d'un contrat de bail, etc.)

## Visite de logement par un client
- une client peut visiter plusieurs logements
- la visite est organisée par au moins deux employés de l'agence et un client
- une vsite se tient à une adresse, une date et une heure précise
- une visite a un identifiant unique

## Utilisateur	
-	Doit pouvoir se connecter pour utiliser l’application
-	A un rôle dans une hiérarchie donnée

## Client	
-	Signe un contrat de bail pour un logement précis
-	Peut signer plusieurs contrats en même temps (logements différents)
-	A un nom, prénom, email (facultative), téléphone1, téléphone2, adresse, date de naissance, lieu de naissance, civilité (Homme ou Femme), 
-	A une ou plusieurs personnes référentes (à contacter au besoin) : ce n’est pas un garant
-	Garant (facultatif)
-	Fournit une copie de pièce identité
-	Un client possède son espace personnel en ligne

## Contrat de bail	
-	Fait le lien entre un client et un logement, supervisé par un utilisateur
-	Ne concerne qu’un client sur un logement en même temps
-	Montant du loyer
-	Montant de la caution (fonction du montant du loyer) : 1, 2, 3, 4, 5, … mois de loyers. L'encaissement de la caution affecte la caisse en opération de type "entrée". Motif : paiement de caution
-	Périodicité de paiement des loyers
-	Date de paiement des futurs loyers (dpfl) : dépend de la périodicité et de la durée du contrat. Se calcule : date de début d’occupation du logement + périodicité de paiement des loyers = dpfl_1. Ensuite on calcule dpfl_2, etc.
-	Date du prochain paiement
-	Date de signature du contrat de bail
-	Date de début d’occupation du logement
-	Date de fin d’occupation du logement
-	Date de fin estimée
-	Charte du locataire, clauses, etc.
-	Période de creux : différence entre la date de début d’occupation et la date de signature du contrat
-	Paiement de creux : montant du loyer / nombre de jour de la période de creux
-	La date de paiement de loyer correspond à la date de début d’occupation du loyer
-	Le mois (ou autre périodicité) de loyer se paie à l’avance
-	Etat du paiement : normal ou anormal. Compare la date du jour avec celle du prochain paiement. Si la date du prochain paiement est inférieure à la date du jour, alors « anomal ». Calcule alors le nombre de jours de retard : date du jour – date de prochain paiement
-	Fin du contrat de bail :
  Soit : "contrat résilié pour continuer" : par exemple suite à une modification de prix, une modificatin de la date de paiement, etc.
 	Soit : "contrat résilié définitement" : un état des lieux est organisé. Les travaux à refaire sont enregistrés dans la table __Travaux__. Le logement passe à l'état "en maintenance".

## Paiement du loyer
-	Concerne un contrat (client + logement)
-	Concerne une période (date début, date fin) : période concerné par le paiement
-	Met à jour la Date du prochain paiement
-	Se fait par un client pour un logement et supervisé par un utilisateur
-	A une date
-	A un montant (facultatif : car déjà présent dans le contrat)
-	A un mode : chèque non autorisé, en espèce, mobile money, virement bancaire, transfert agence (Western union, Money gram, …)
-	Le client reçoit un reçu par email et/ou papier imprimé
-	Est un paiement complet ou une avance de paiement
-	Met à jour la caisse en une opération de type "entrée". Motif : paiement de loyer

## Travaux	
-	A un type de travaux : construction, maintenance, maintenance après état des lieux, etc.
-	A une date début et de fin (estimation)
-	A une date de fin effective
-	A une nature de travaux : plomberie, menuiserie, etc.
-	A un montant
-	Possède le nom d'intervenant
-	Concerne un logement
-	Concerne un con
-	Met à jour la caisse. Le décaissement affecte la caisse en opération de type "sortie". Motif : travaux sur logement ...

## Incident	
-	Concerne un ou plusieurs contrats (contient les infos du locataire et du logement)
-	Contient un descriptif de la situation

<!-- Liste des impayés -->

## Caisse / opération de caisse
-	A un type : entrée (paiement de caution, de loyer) ou sortie d’argent (travaux, salaire employés, etc.)
-	A une date
-	A un montant
-	A un motif : paiement de caution/loyer, travaux sur un logement précis, etc.
-	Concerne un contrat de bail précis (information facultatif : par exemple dans le cas d'un paiement de salaire)
-	A un descriptif (facultatif)

<!-- Généralités :
-	Chaque document édité contient la date du jour de l’édition
-	Tenir à jour une liste de client en défaut de paiement. Un code tourne chaque nuit : parcourir tous les contrats et alimenter la base de données (table) des client en défaut de paiement. Date du prochain paiement…
- Le changement du montant de loyer, de périodicité de paiement font l'objet d'un nouveau contrat.
-->
