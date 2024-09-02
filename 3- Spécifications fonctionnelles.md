# Spécifications fonctionnelles

Les fonctionnalités ci-dessous sont regroupés par "bloc de fonctionnalité". Ce dernier se trouve dans un fichier à part.

Les principales entités : logement, utilisateur (employé de l’agence), client, contrat de bail, paiement de loyer, travaux

## Logement	
-	A un état : en travaux, prêt à la location, en location, en maintenance
-	Pour être loué, le logement doit être dans l’état « prêt à la location »
-	Peut faire l’objet d’un ou plusieurs travaux en même
-	Peut être loué à seul client en même temps
-	A des caractéristiques : adresse (numéro de rue/avenue, quartier, ville, pays), plan (salon, nombre de chambre, douche/toilette, cuisine, véranda, autre), visuels
-	A un identifiant unique
-	Dates clés : en fonction de l’état (pour chaque changement, on note la date) : livré par l’entreprise de BTP, …
-	Descriptif

## Visite pour un client
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

## Contrat de bail	
-	Fait le lien entre un client et un logement, supervisé par un utilisateur
-	Ne concerne qu’un client sur un logement en même temps
-	Montant du loyer
-	Montant de la caution (fonction du montant du loyer) : 1, 2, 3, 4, 5, … mois de loyers
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

## Paiement	
-	Concerne un contrat (client + logement)
-	Concerne une période (date début, date fin) : période concerné par le paiement
-	Met à jour la Date du prochain paiement
-	Se fait par un client pour un logement et supervisé par un utilisateur
-	A une date
-	A un montant
-	A un mode : chèque non autorisé, en espèce, mobile money, virement bancaire, transfert agence (Western union, Money gram, …)
-	Le client reçoit un reçu par email et/ou papier imprimé
-	Est un paiement complet ou une avance de paiement
-	Met à jour la caisse

## Travaux	
-	Type de travaux : construction, maintenance
-	Date début et de fin (estimation)
-	Date de fin effective
-	Types : plomberie, menuiserie, etc.
-	Montant
-	Nom intervenant
-	Logement concerné
-	Met à jour la caisse

## Incident	
-	Contrat (contient les infos du locataire et du logement)
-	Locataire
-	Descriptif

<! -- Liste des impayés -->

## Caisse 	
-	Type d’opération : entrée ou sortie d’argent
-	Date
-	Montant
-	Contrat concerné
-	descriptif

<!-- Généralités :
-	Chaque document édité contient la date du jour de l’édition
-	Tenir à jour une liste de client en défaut de paiement. Un code tourne chaque nuit : parcourir tous les contrats et alimenter la base de données (table) des client en défaut de paiement. Date du prochain paiement…
-->
