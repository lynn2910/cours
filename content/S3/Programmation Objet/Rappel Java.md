---
title: Rappel JAVA
draft: false
tags:
---
## 1. Les classes

### 1.1 Vocabulaire

**Classe:** Modèle d'une chose/d'un concept. C'est un ensemble de propriétés/fonctionnalités.

**Attribut:** ==Une== des propriétés de la classe.

**Méthode:** ==Une== fonctionnalité de la classe.

**Membre (de classe):** Attribut ou méthode

**Objet:** Instance d'une classe. (Un exemplaire)

**L'instanciation:** Processus de construction d'un objet

**Constructeur:** Méthode particulière qui décrit comment créer un objet (initialiser les/des attributs).

**Destructeur:** Méthode particulière pour supprimer un Objet

> [!Warning] Constructeur & Destructeur
> Le cas général est plusieurs constructeurs et un seul destructeur (ex. C++), mais en Java, les destructeurs n'existent pas ([IBM garbage collector](https://www.ibm.com/topics/garbage-collection-java))

### 1.2 Déclaration/Définition

Le code Java sera dans les fichiers avec l'extension `.java`.

Les droits d'accès des membres sont spécifiés par:
- `public`: Tout objet peut y accéder, y compris sa classe
- `protected`: Tout objet de la même casse ou de ses sous-classes peut les manipuler. (S'étend également aux classes du package)
- `private`: Uniquement un objet de la classe peut le manipuler.

### 1.3 Instanciations

Instancier, c'est appeler un des constructeurs de la classe.


