---
title: Langages et frameworks
draft: false
tags:
---
Ce dossier contient des informations, ressources et cours sur différents langages de programmations.

## Liste de frameworks et outils


| Nom                       | Utilité            | Couverture des ressources |
| ------------------------- | ------------------ | ------------------------- |
| [[Tailwind\|TailwindCSS]] | Style de pages web | 0%                        |


## Liste des langages 

| Langage (avec lien) | Utilité                                                             | Type de langage | Couverture des ressources | Paradigme                              |
| ------------------- | ------------------------------------------------------------------- | --------------- | ------------------------- | -------------------------------------- |
| [[Rust]]            | Bas-niveau, intégré, programmes nécessitant des hautes performances | Compilé         | 0%                        | Fonctionnel, Orienté Objet, Impératif  |
| [[Typescript]]      | APIs                                                                | Interprété      |                           | Fonctionnel, Orienté Object, Impératif |
| [[Java]]            | Bas-niveau, APIs, algorithmie, programmation orientée objet         | Compilé (JVM)   | 0%                        | Orienté Objet                          |
| [[Javascript]]      | Web, APIs                                                           | Interprété      | 0%                        | Fonctionnel, Orienté Objet, Impératif  |
| [[Python]]          | APIs, Algorithmies, Scripting                                       | Interprété      | 0%                        | Fonctionnel, Orienté Objet, Impératif  |

---

## Les paradigmes

*Ressources : [fr.wikipedia.org](https://fr.wikipedia.org/wiki/Paradigme_(programmation))*

Un paradigme de programmation, c'est un peu comme une recette de cuisine : il fournit une méthode, un ensemble de règles et de concepts pour structurer et organiser le code. Chaque paradigme offre une perspective unique sur la résolution de problèmes informatiques et influence la façon dont les développeurs conçoivent et implémentent leurs logiciels.

### Pourquoi tant de paradigmes ?

Tout comme il existe différentes langues dans le monde, chaque avec sa propre grammaire et sa propre culture, les langages de programmation ont aussi leurs propres paradigmes. Ces différents paradigmes répondent à des besoins spécifiques et à des types de problèmes particuliers. Certains sont mieux adaptés à la création d'interfaces utilisateur, d'autres à la manipulation de données complexes, et d'autres encore à la réalisation de calculs scientifiques.

### Les principaux paradigmes

#### 1. **Programmation Impérative**

Le paradigme impératif est le plus ancien et le plus intuitif. Il consiste à donner à l'ordinateur une suite d'instructions à exécuter séquentiellement pour modifier l'état du programme. C'est comme donner une recette de cuisine : chaque étape doit être suivie dans l'ordre.

**Caractéristiques clés :**
- **États modifiables :** Les variables peuvent changer de valeur au cours de l'exécution du programme.
- **Instructions séquentielles :** Les instructions sont exécutées ligne par ligne.
- **Structures de contrôle :** Les conditions (if/else) et les boucles (for, while) permettent de contrôler le flux d'exécution.

**Exemple :**
```py
nombre = 10
nombre = nombre + 5
print(nombre)  # Affichera 15
```

---
#### 2. Programmation Orienté Objet (POO)

La POO est un paradigme qui modélise le monde réel en termes d'objets. Chaque objet possède des propriétés (attributs) et des comportements (méthodes).

**Caractéristiques clés :**
- **Objets :** Des instances de classes qui encapsulent des données et des comportements.
- **Classes :** Des modèles qui définissent les propriétés et les méthodes des objets.
- **Héritage :** Un mécanisme permettant de créer de nouvelles classes à partir de classes existantes.
- **Polymorphisme :** La capacité d'un objet à prendre plusieurs formes.

**Exemple :**
```java
class Voiture {
    String marque;
    int vitesseMax;

    public void accelerer() {
        // Code pour accélérer la voiture
    }
}
```

*Voir [[Programmation Objet]]*

---
#### 3. Programmation fonctionnelle

La programmation fonctionnelle traite le calcul comme l'évaluation de fonctions mathématiques. Les fonctions sont des citoyens de première classe et les données sont immuables.

**Caractéristiques clés :**
- **Fonctions pures :** Des fonctions qui ne modifient pas l'état externe et retournent toujours le même résultat pour les mêmes entrées.
- **Immuabilité :** Les données ne peuvent pas être modifiées après leur création.
- **Récursivité :** Une technique de programmation où une fonction s'appelle elle-même.
- **Higher-order functions :** Des fonctions qui prennent d'autres fonctions en argument ou qui en retournent.

**Exemple :**
```js
const nombres = [1, 2, 3, 4, 5];
const nombresPairs = nombres.filter(nombre => nombre % 2 === 0);
```

---
#### 4. Programmation procédurale

La **programmation procédurale** est un paradigme de programmation qui se concentre sur l'exécution séquentielle d'instructions pour réaliser une tâche. C'est un des premiers paradigmes à avoir été développé et il reste très utilisé aujourd'hui, notamment comme base pour d'autres paradigmes comme la programmation orientée objet.

**Caractéristiques clés :**
- **Séquence :** Les instructions sont exécutées l'une après l'autre, dans l'ordre où elles sont écrites.
- **Sélection :** Des structures conditionnelles (si/sinon) permettent de choisir entre différentes branches d'exécution en fonction de certaines conditions.
- **Répétition :** Les boucles (pour, tant que) permettent de répéter un bloc d'instructions plusieurs fois.
- **Procédures (ou fonctions) :** Des blocs d'instructions regroupés sous un nom et pouvant être appelés à plusieurs reprises dans le programme.

**Exemple:**
```c
#include <stdio.h>

void saluer(char *nom) {
    printf("Bonjour, %s !\n", nom);
}

int main() {
    char nom[20];
    printf("Quel est votre nom ? ");
    scanf("%s", nom);
    saluer(nom);
    return 0;
}
```

#### 5. Programmation logique

La programmation logique consiste à décrire un problème à résoudre sous forme de faits et de règles. Le programme cherche alors à trouver une solution en déduisant de nouvelles informations à partir de ces faits et règles.

**Caractéristiques clés :**
- **Faits :** Des déclarations qui sont toujours vraies.
- **Règles :** Des implications qui permettent de déduire de nouvelles informations.
- **Unification :** Un mécanisme qui permet de trouver des substitutions pour les variables afin de rendre les termes égaux.

**Exemple ([prolog](https://fr.wikipedia.org/wiki/Prolog)):**
```prolog
pere(jean, marc).
pere(marc, paul).
homme(jean).
homme(marc).
homme(paul).

homme(X) :- pere(X, Y), homme(Y).
```