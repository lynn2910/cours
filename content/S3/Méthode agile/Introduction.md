---
title: Introduction
draft: false
tags: 
aliases: []
---
## Préambule

Chaque logiciel possède un cycle de vie:
- Identification d'un besoin
- Création d'un produit
- Livraison
- Maintenance

## Cascade (1966)

![[cascade.jpeg]]

Dans ce système, il faut valider l'étape précédente. Cela permet de remettre en cause l'étape précédente si il manque des choses/si elles ne vont pas.

Cette structure permet de revenir en arrière en cas de nécessité de changement/remaniements nécessaires dans les étapes précédentes.

### Origine

Ce module a été conçu au départ pour le BTP (Batiments-Travaux publics).
Il a été réutilisé pour les projets informatiques.

### Inconvénients

L'inconvénient principal est qu'on ne peut pas remettre en cause l'étape de conception si on est à l'étape des tests.

Chaque phase va générer son ensemble de documents qui vont être transmis à l'étape suivante, générant une certaine lourdeur du process.


## Cycle en V (1980)

![[Capture-decran-2023-06-14-185147.png]]

### Objectif

L'objectif du cycle en V est d'améliorer le système en cascade en ajoutant des phases de tests.

Les tests vont permettre de mettre en évidence les problèmes dans les phases précédentes.

### Critiques

Il y a plusieurs critiques:
- Validation d'étapes basées sur des revues de documents
- Production de nombreux documents
- Etapes séquentielles
- Difficulté de prise en compte du changement

Ces difficultés viennent principalement de la différence entre le BTP et l'informatique:
- La durée de vie du produit
- Evolution du besoin
- Périmètre fonctionnel constant/changeant

## D'autres cycles

D'autres cycles ont été développés:
- Cycle par prototypage (années 80)
- Cycle en Y (années 90)

### Problèmes

Il y a eu de nombreux problèmes:
- Attentes et retards
- Transmissions d'informations
- Processus et fonctionnalités inutiles
- Effet tunnel

## Méthodes Agiles (2000)

[agilemanifesto](https://agilemanifesto.org/)

4 valeurs ont été définies:
- **Les individus et leurs interactions** plus que les processus et les outils  
- **Des logiciels opérationnels** plus qu’une documentation exhaustive  
- **La collaboration avec les clients** plus que la négociation contractuelle  
- **L’adaptation au changement** plus que le suivi d’un plan

### Principes sous-jacents

12 principes:
1. Notre plus haute priorité est de **satisfaire le client** en livrant rapidement et régulièrement des fonctionnalités à grande valeur ajoutée.
2. **Accueillez positivement les changements de besoins**, même tard dans le projet. Les processus Agiles exploitent le changement pour donner un avantage compétitif au client.
3. **Livrez fréquemment** un logiciel opérationnel avec des cycles de quelques semaines à quelques mois et une préférence pour les plus courts.
4. Les utilisateurs ou leurs représentants et les développeurs doivent **travailler ensemble quotidiennement** tout au long du projet.
5. Réalisez les projets avec des personnes motivées. Fournissez leur l’environnement et le soutien dont ils ont besoin et **faites leur confiance** pour atteindre les objectifs fixés.
6. La méthode la plus simple et la plus efficace pour transmettre de l’information à l'équipe de développement et à l’intérieur de celle-ci est le **dialogue en face à face**.
7. Un **logiciel opérationnel est la principale mesure d’avancement**.
8. Les processus Agiles encouragent **un rythme de développement soutenable**. Ensemble, les commanditaires, les développeurs et les utilisateurs devraient être capables de maintenir indéfiniment un rythme constant.
9. Une **attention continue à l'excellence technique** et à **une bonne conception** renforce l’Agilité.
10. **La simplicité** – c’est-à-dire l’art de minimiser la quantité de travail inutile – est essentielle.
11. Les meilleures architectures, spécifications et conceptions émergent **d'équipes autoorganisées**.
12. À intervalles réguliers, l'équipe réfléchit aux moyens de **devenir plus efficace**, puis règle et modifie son comportement en conséquence.

### Méthodes

On en trouve essentiellement deux qui sont beaucoup utilisées:
- XP: eXtrem Programming (Kent Beck, Ward Cunningham, Ron Jeffries)
- SCRUM (Ken Schwaber, Jeff Sutherland)

