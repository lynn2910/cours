---
title: Introduction
draft: false
tags: 
aliases: []
---
## Préambule

Chaque logiciel possède un cycle de vie :
- Identification d'un besoin
- Création d'un produit
- Livraison
- Maintenance.

## Cascade (1966) : issue du BTP


Le **modèle en cascade** est une méthodologie de gestion de projet séquentielle, linéaire et descendante. Chaque phase du projet doit être complétée avant de passer à la suivante. Cette approche, bien qu'historique, a longtemps été un standard dans l'industrie, notamment dans le BTP et l'informatique.

![[cascade.jpeg]]

**Les phases typiques du modèle en cascade :**
1. **Conception :** Définition des besoins, spécifications fonctionnelles et techniques.
2. **Développement :** Réalisation du produit ou du service.
3. **Tests :** Vérification du fonctionnement et de la conformité aux spécifications.
4. **Déploiement :** Mise en production.
5. **Maintenance :** Corrections, évolutions et support.
### Origine

Ce module a été conçu au départ pour le BTP (Bâtiments-Travaux publics).
Il a été réutilisé pour les projets informatiques.

### Inconvénients

Parmi les inconvénients, on retrouve :
- **Rigidité :** Difficulté à revenir en arrière une fois une phase terminée. Les changements sont coûteux et peuvent retarder le projet.
- **Manque de flexibilité :** Le modèle est peu adapté aux projets complexes, évolutifs ou aux exigences changeantes.
- **Risque d'échec :** Les problèmes ne sont souvent détectés qu'en phase de test, ce qui peut entraîner des coûts supplémentaires et des retards.
- **Long délai de livraison :** Chaque phase doit être complétée avant de passer à la suivante, ce qui allonge la durée du projet.
- **Difficulté à gérer les projets complexes :** Les interdépendances entre les différentes phases peuvent être difficiles à gérer.


## Cycle en V (1980)

Le **modèle en V** est une évolution du modèle en cascade, conçu pour intégrer dès le début du projet une dimension qualité et vérification. Il s'articule autour d'une structure en forme de V,

![[Capture-decran-2023-06-14-185147.png]]

**Les phases typiques du modèle en V :**
- **Conception :**
    - **Conception système :** Définition des besoins globaux.
    - **Conception architecturale :** Structure générale du système.
    - **Conception détaillée :** Spécifications détaillées de chaque module.
- **Réalisation :**
    - **Codage :** Écriture du code.
    - **Tests unitaires :** Vérification de chaque module individuellement.
- **Validation :**
    - **Tests d'intégration :** Vérification de l'interaction entre les modules.
    - **Tests de validation :** Vérification de la conformité aux spécifications.
    - **Tests d'acceptation :** Validation par le client.
### Objectif

L'objectif du cycle en V est d'améliorer le système en cascade en ajoutant des phases de tests.

Les tests vont permettre de mettre en évidence les problèmes dans les phases précédentes.

### Critiques

Il y a plusieurs critiques :
- Validation d'étapes basées sur des revues de documents
- Production de nombreux documents
- Étapes séquentielles
- Difficulté de prise en compte du changement.

Ces difficultés viennent principalement de la différence entre le BTP et l'informatique :
- La durée de vie du produit
- Évolution du besoin
- Périmètre fonctionnel constant/changeant.

## D'autres cycles

D'autres cycles ont été développés avec la méthode *Agile*:
- Cycle par prototypage (années 80)
- Cycle en Y (années 90).

### Problèmes

Il y a eu de nombreux problèmes :
- Attentes et retards
- Transmissions d'informations
- Processus et fonctionnalités inutiles
- [Effet tunnel](https://fr.wikipedia.org/wiki/Effet_tunnel_(gestion_de_projet)).

## Méthodes Agiles (2000)

[Manifeste pour le développement Agile de logiciels](https://agilemanifesto.org/)
Par : Kent Beck, Mike Beedle, Arie van Bennekum, Alistair Cockburn, Ward Cunningham, Martin Fowler, James Grenning, Jim Highsmith, Andrew Hunt, Ron Jeffries, Jon Kern, Brian Marick, Robert C. Martin, Steve Mellor, Ken Schwaber, Jeff Sutherland, Dave Thomas

4 valeurs ont été définies :
- **Les individus et leurs interactions** plus que les processus et les outils  
- **Des logiciels opérationnels** plus qu’une documentation exhaustive  
- **La collaboration avec les clients** plus que la négociation contractuelle  
- **L’adaptation au changement** plus que le suivi d’un plan

### Principes sous-jacents

Douze principes :
1. Notre plus haute priorité est de **satisfaire le client** en livrant rapidement et régulièrement des fonctionnalités à grande valeur ajoutée.
2. **Accueillez positivement les changements de besoins**, même tard dans le projet. Les processus Agiles exploitent le changement pour donner un avantage compétitif au client.
3. **Livrez fréquemment** un logiciel opérationnel avec des cycles de quelques semaines à quelques mois et une préférence pour les plus courts.
4. Les utilisateurs ou leurs représentants et les développeurs doivent **travailler ensemble quotidiennement** tout au long du projet.
5. Réalisez les projets avec des personnes motivées. Fournissez-leur l’environnement et le soutien dont ils ont besoin et **faites leur confiance** pour atteindre les objectifs fixés.
6. La méthode la plus simple et la plus efficace pour transmettre de l’information à l'équipe de développement et à l’intérieur de celle-ci est le **dialogue en face à face**.
7. Un **logiciel opérationnel est la principale mesure d’avancement**.
8. Les processus Agiles encouragent **un rythme de développement soutenable**. Ensemble, les commanditaires, les développeurs et les utilisateurs devraient être capables de maintenir indéfiniment un rythme constant.
9. Une **attention continue à l'excellence technique** et à **une bonne conception** renforce l’Agilité.
10. **La simplicité** – c’est-à-dire l’art de minimiser la quantité de travail inutile – est essentielle.
11. Les meilleures architectures, spécifications et conceptions émergent **d'équipes autoorganisées**.
12. À intervalles réguliers, l'équipe réfléchit aux moyens de **devenir plus efficace**, puis règle et modifie son comportement en conséquence.

### Méthodes

On en trouve essentiellement deux qui sont beaucoup utilisées :
- XP: eXtrem Programming (Kent Beck, Ward Cunningham, Ron Jeffries)
- SCRUM (Ken Schwaber, Jeff Sutherland)

## Quels sont les avantages et les inconvénients de l'Agile par rapport au Waterfall ?

### Agile

#### Avantages
1. Flexibilité pour répondre au marché et aux nouvelles informations
2. L'équipe de mise en œuvre a de la marge pour la résolution créative de problèmes
3. Équipes auto-organisées et allocation des ressources
4. Mises à jour fréquentes et valeur accrue pour le client
5. Cadence rigide, flexibilité des délais

#### Inconvénients
1. Une planification souple peut conduire à un produit final imprévisible et à des glissements de dates
2. Susceptible de manquer de concentration et de réactions impulsives d'un Sprint à l'autres
3. Rythme implacable
4. Des exigences de test souples peuvent laisser passer des bugs
5. Pas de possibilité de faire des changements pendant un Sprint

### Waterfall

#### Avantages
1. Dérive minimale de la portée du projet
2. Un produit final prévisible et bien spécifié
3. Rôles et responsabilités bien définies 
4. Versions peu fréquentes qui peuvent être soigneusement déployées et communiquées aux utilisateurs et au marché
5. Plans de projet précis et délais fermes

#### Inconvénients
1. Manque de flexibilité après une spécifications
2. Moins d'opportunités pour corriger le cap
3. Trop d'écarts entre les innovations atteignant le marché
4. Trop long avant que les bugs ne soient découverts puisque les tests n'ont lieu qu'une fois le grand projet terminé
5. Processus bureaucratique de gestion des changements

## Le sprint

En méthode agile, un sprint est une **itération temporelle courte et fixe** au cours de laquelle une équipe de développement travaille sur un ensemble de fonctionnalités bien défini. C'est comme une mini-projet à part entière, avec un début et une fin précis. L'objectif est de livrer un incrément de produit fonctionnel à la fin de chaque sprint, permettant ainsi de montrer rapidement de la valeur au client et d'obtenir un feedback régulier. La durée d'un sprint est généralement fixée entre une et quatre semaines, et elle est choisie en fonction de la complexité du projet et des besoins de l'équipe.

![[Sprint.webp]]

Tout au long du Sprint :
- L’objectif du sprint n’est pas mis en péril
- La qualité n’est pas compromise
- Le [backlog de produit](https://chef-de-projet.fr/product-backlog/) est affiné si nécessaire
- La portée peut être définie et renégociée avec le [Product Owner](https://chef-de-projet.fr/metiers/product-owner/) à mesure que des informations supplémentaires sont disponibles.

## Fonctionnement du sprint

Un sprint est une période définie où une équipe Scrum se consacre à la réalisation d'un ensemble de tâches bien précis. Chaque sprint est comme un mini-projet avec un objectif clair.
- **Planification rigoureuse:** Au début de chaque sprint, l'équipe sélectionne les tâches à réaliser et définit un plan d'action.
- **Travail quotidien:** Les membres de l'équipe se réunissent quotidiennement pour faire le point sur leurs avancées et lever les obstacles.
- **Livraison incrémentale:** À la fin du sprint, l'équipe présente le résultat de son travail : un produit fonctionnel, même s'il est partiel.
- **Amélioration continue:** La dernière étape consiste à analyser ce qui s'est bien passé et ce qui pourrait être amélioré pour les prochains sprints.

Chaque sprint est l'occasion de :
- **Livrer de la valeur:** Le produit s'enrichit à chaque sprint.
- **S'adapter:** L'équipe peut ajuster sa roadmap en fonction des retours des utilisateurs et des évolutions du marché.
- **Améliorer:** L'équipe apprend de ses expériences et met en place des actions pour améliorer son processus de travail.

## SCRUM

### Rôles
- Product Owner
- Scrum Master
- Développeur
- Parties prenantes

### Artefacts
- Backlog
- Plan de release
- Plan de sprint
- Burndown
- Définition de fini
- Liste des obstacles

