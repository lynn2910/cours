---
title: Présentation SAE
draft: false
tags: 
aliases:
  - sae
---
## Objectifs

Réalisation d'une appli. en couvrant des compétences du S3-S4

Choix entre deux types de sujet:
1. **Fortement orienté web + un peu de traitement de données**
2. Equilibre entre traitement données et web

Travail en équipe de 5 étudiants (voir 4 selon effectifs)

Présentation & rendus fin de semestres.

## Sujets

### Sujet 1: orienté web

Gestion d'un évènement:

#### Architecture logicielle
- backend: API en NodeJS + Base de donnée relationnelle type mysql
- frontend: VueJS

#### Fonctionnalités principales:
- présentation évènement: Permet de présenter l'évènement
- plan interactif avec info-bulle: plus complexe
- prestataires proposant des "services" (livre d'or, réservation...):
	- Propose des services/prestations sur l'application
	- ex. Prestataires pour le FIMU: planning, boutiques, billeterie; services **en ligne**
- partie administration: gestion des présentation, plan, prestataires
- visualisation graphique de données issues de la base de donnée
- 2 langues (français + anglais)

### Authentification

Au minimum 3 types d'utilisateurs:
- non-authentifié (=public): accès prestataires/plan/services presta
- prestataire: gestion présentation/services proposés (+ visu graphiques?)
- orga: gestion présentation, plan, prestataires, + visu graphiques

### Remarques

- Prestataires = personne morale/physique proposant des services dans l'appli
- service = processus interactif sur le front-end permettant de se renseigner, planifier, réagir, etc... à une activité effectuée lors de l'évènement.
	- consulter un planning
	- réserver une place/nourriture
	- laisser un commentaire,...

### Sujet 2: site d'information "statistique"

#### Architecture logicielle
- backend: scripts python + API en NodeJS + base de donnée relationnelle type mysql
- Frontend: VueJS

#### Fonctionnalités principales
- collecte & nettoyage (si possible dynamique) de données multi-sources,
- intégration du résultat en base de donnée
- créer stats. & graphiques à partir d'un et plusieurs jeux de données,
- page d'accueil (texte, carrousel, ...),
- visualisation paramétrée de statistiques & graphiques
- partie admin: gestion accueil, utilisateurs, données...
- 2 langues (français + anglais)

#### Authentification

Au minimum 3 types d'utilisateurs:
- non-authentifié (=public): accès accueil, enregistrement,
- enregistré: visu stats & graphiques
- orga: gestion accueil, utilisateurs, données, ...

#### Remarques
- Possibilités de mise à jour des donnes (via scripts ou mieux, via admin.)
- Dans certains cas, simulation de jeux de données
- Obligation de faire des croisements entre les jeux de donnés, notamment pour trouver des corrélations
- Pas d'analyse de données via IA demandée (= 3ème années)

## Organisation

Planning prévisionnel:
- 09/09: Lancement de la SAE
- jusqu'au 16/09: création des groupes & choix des sujets, envoi à sdomas@univ-fcomte.fr
- 19/09: mini-présentation des sujets choisis par chaque groupe
- chaque semaine: moitié des groupes en suivi de projet (F. Ambert)
- Fin S3 & S4: soutenance + rendu des délivrables (selon sujet & semestre)

Une semaine sur deux, cours avec F. Ambert sur la méthode agile pour la moitié des groupes.

Référents principaux:
- API & NodeJS: J. Azar
- Base de donnée: J. Azar, D. Laiymani
- Front-end: S. Domas
- Collecte et traitement des données: H. Noura
- Suivi projet: F. Ambert
\+ d'autres: K. Deschinkel (placement auto & optimisation), I. Couturier & C. Paterlini (anglais & comm), ...

Articles sur cours-info:
- notes d'organisation,
- Descriptif et attendus pour chaque projet
- supports de cours de SAE

**Page cours-info:** [cours-info](https://cours-info.iut-bm.univ-fcomte.fr/index.php/menu-cours-s3/sae-dev-appli-avec-bdd)
