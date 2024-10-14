---
title: API Rest
draft: false
tags:
---
## Précédents

Avant [[#Rest]], l'architecture [SOAP](https://www.ibm.com/docs/fr/cics-ts/6.x?topic=format-soap-web-services-architecture) était très connue

## RPC
## Rest

**API:** Application Programmable Interface

**Rest** est un style d'architecture logiciel

### Les types de requêtes

On retrouve différents types de requêtes très utilisées :
- GET: obtenir des données
- POST: créer une nouvelle réponse ou donnée
- PATCH : Mettre à jour des données.
- DELETE: Supprimer des données

On retrouve également de nombreuses autres méthodes (voir [mdn](https://developer.mozilla.org/en-US/docs/Web/HTTP/Methods))

Les méthodes `GET`, `HEAD`, `OPTION` sont safes, car ils ne modifient pas d'informations, mais des méthodes telles que `POST`, `DELETE`, `PATCH` ou encore `PUT` ne sont pas safe, parce qu'elles modifient des données.

### Statelessness

Les serveurs ne sont pas autorisés à stocker des données relatives au client. Aucun état de session ne doit être stocké coté serveur.

**Statelessness** exige que chaque requête du client au serveur contienne toutes les informations nécessaires pour comprendre et compléter la requête.

Le serveur ne peut pas tirer parti des informations de contexte précédemment stockées sur le serveur. Pour cette raison, l'application cliente doit conserver entièrement l'état de la session.

### Le cache

La contrainte "cacheable" exige qu'une réponse s'étiquette implicitement ou explicitement comme *cacheable* ou *non cacheable*.

### Système en couche

l'API doit être séparé en plusieurs fichiers, notamment en séparant les méthodes de la base de donnée. On va également séparer la logique dans les controllers.

### Uniformité

Il y a 4 règles:
- Identification of **resources**
- Manipulation of resources through representations
- Self-descriptive 

