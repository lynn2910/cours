---
title: SAE
draft: false
tags:
---

# Architecture

![[SAE_BUT5_A_archiglobale 1.png]]
# Mission

Faire évoluer l'architecture. Chaque groupe va reprendre un code et travailler avec celui-ci.

On doit créer un proof-of-concept que l'application est faisable, et intéressante.

> [!tip] Il faudra réutiliser des bouts de code des cours

# Sujet

Le sujet est **libre**. Le TP d'exemple et un système de domotique, mais plusieurs sujets sont faisables :
- surveillance des cours d'eau ;
- concours d'athlétisme, football ;
- etc.

# Consignes

1. Utiliser des microcontrôleurs (cours fin septembre) avec des capteurs (température, luminosité, caméras, boutons, écrans LCD, ...) ;
2. le serveur TCP en Java, multi-threadé, doit stocker les données récoltées dans une base de donnée ;
3. la base de donnée doit être MongoDB (non-relationnelle), et être capable d'envoyer des données de Java à MongoDB ;
4. le serveur Java peut envoyer des données à une API Node, qui les stockeras dans MongoDB elle-même ;
5. l'API NodeJS va servir de médiation entre MongoDB et le serveur Java, mais aussi de connecter à un frontend afin de visualiser les données (cette partie n'est pas fondamentale) ;
6. le frontend devrait aussi pouvoir configurer certains trucs sur les microcontrôleurs (ex. pour un système pour piscine, pouvoir changer le temps de mise à jour de la prise de température) ;
7. un dispositif mobile (Téléphone, tablette, Android ou iOS) doit permettre de capturer un média (image(s), son, vidéo, coordonnées GPS, ...), et envoie le résultat à un serveur, qui va traiter le(s) média(s) ;
8. le serveur d'analyse média peut utiliser des modèles d'IA ou des méthodes de traitement algorithmique, il peut utiliser des librairies, systèmes pré-faits etc... Il enverra au serveur TCP les données, etc... ;
9. l'API NodeJS doit pouvoir également traiter des données avec des scripts Python d'IA. Grâce au frontend, on va dire "je veux classifier ...", sélectionner les critères etc... et l'api va lancer des scripts qui vont faire tourner des modèles, et retourner des résultats à l'API, qui va le renvoyer au frontend.
10. Les modèles d'IA peuvent être existants, ou utiliser un modèle et le réentraîner, ou créer leur propre modèle.

> [!tip] pouvoir configurer les paramètres et faire du traitement d'IA à distance (frontend) est important.

> [!danger] Les modèles d'IA dans l'API NodeJS est une partie importante du projet

# Problématiques systèmes

Les services seront mis en place avec Docker.

Des systèmes d'intégration continue et de déploiement seront également mis en place.

# Conseils

## Configuration des microcontrôleurs

Prévoir que les microcontrôleurs demandent de mettre à jour leur configuration à chaque requête.

# Organisation

## 1. Constitution des groupes et choix du sujet

- Groupes de 5 étudiants, ou 4 si pas possible autrement ;
- trouver une idée de sujet qui correspond aux attentes du projet ;

> [!danger] A rendre le 26 septembre à `sdomas@univ-fcomte.fr`

## 2. Suivi du projet

La gestion d'équipe et la gestion projet sont laissées libres pour gérer le projet.

C'est aux étudiants de gérer le projet comme ils veulent (méthode agile, semi-agile, chef de projet ?, ...)

Les profs s'en fichent, ils veulent juste des résultats.

## 3. Professeurs référents

- **S. Domas :** microcontrôleurs, client/serveur Java, programmation mobile hybride (JS), front-end et back-end, MongoDB
- **G. Perrot** : analyse multimédia, programmation mobile native Android ;
- **D. Layimani :** IA, programmation mobile native iOS,
- **F. Ambert :** gestion de projet
- **F. Lassabe :** conteneurisation, automatisation

## 4. Planning et attendus

### Disponibilité des sources existantes

Les sources seront disponibles début octobre sur `cours-info`. L'objectif est que les grands concepts soient vus avant.

### 26/09/2025. Groupe + sujet

###  12/10/2025. Rapport initial

Un premier rapport de 4-5 pages est à rendre, il devra inclure :
- le contexte applicatif ;
- les scénarios d'utilisation ;
- les fonctionnalités principales ;
- le matériel utilisé ;
- etc.

Il doit être envoyé au plus tard le **dimanche 12 octobre**.

> [!info] plus de détails sur [cours-info](https://cours-info.iut-bm.univ-fcomte.fr/index.php/menu-lpsil/sae-dev-appli-avec-bdd/2512-delivrable-n-1-rapport-initial)

### 09/01/2026. Délivrable S5

L'évaluation consiste en :
- une soutenance avec démo et le bilan d'avancement. Les diapos devront être déposées sur Moodle pour le `vendredi 9 janvier à 18h` ;
- l'entièreté du code de l'application, également à déposer sous Moodle pour la même date.

### ?. Délivrable S6

> [!todo] non précisé

