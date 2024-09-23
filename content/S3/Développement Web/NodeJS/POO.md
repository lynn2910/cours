---
title: POO
draft: false
tags:
---
> [!Success] Note sur la POO
> JavaScript possèdent de nombreuses similitudes avec Java.
> Pour voir les bases de la POO en Java, voir [[Rappel Java]].

> [!Warning] Note sur les méthodes surchargées
> Les méthodes surchargées n'existent **PAS** en JavaScript.

Référence: [mdn](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Classes)
## Objets/Classes

Nous avons vu en Java que les Objets sont des des exemplaires d'une classe.

En JavaScript, **TOUT** est objet. String, Booléen, Nombre, ... 
L'avantage de cette structuration est qu'ils possèdent un ensemble de caractéristiques communes à tous.

Pour déclarer une classe en JS, on pourra la déclarer de cette manière:
```js
class Voiture {
	model;
	kilometrage;
	reservoir = { max: 35, actuel: 35 }

	constructor(model, kilometrage, essenceMax = 35){
		this.model = model;
		this.kilometrage = kilometrage;
		this.reservoir.max = essenceMax;
	}

	faitPlein(essenceAjoute = null){
		if (!essenceAjoute){
			// essenceAjoute = null, donc on considère qu'on vas au max du reservoir
			this.reservoir.actuel = this.reservoir.max;
		} else {
			this.reservoir.actuel = Math.min(
				this.reservoir.max,
				this.reservoir.actuel + essenceAjoute
			);
		}
	}

	estVide(){
		return this.reservoir.actuel <= 0;
	}

	// Roule K kilometres avec une consommation fixe. L'essence consommée sera retirée
	rouler(kilometres, conso){
		this.kilometrage += kilometres;
		
		this.reservoir.actuel = Math.min(
			this.reservoir.actuel - (conso * kilometrage),
			0
		)
	}
}
```

On peut alors déclarer une voiture de cette manière:
```js
const voiture = new Voiture("Hyundai i10", 62679, 35);
```

Il est alors possible, tout comme en Java, d'utiliser les méthodes. On peut également accéder et modifier les propriétés en toute impunité; le concept d'attributs et méthodes privés/protégés n'existe pas en JavaScript.

### Prototypes

Avantage de Javascript: Les **prototypes**

Un `Prototype`, c'est un **object** contenant l'ensemble des méthodes déclarées pour cet objet.

De sorte que, pour ma classe `Voiture`:
```js
> Voiture.prototype

Object { … }
- constructor: class Voiture { constructor(model, kilometrage, essenceMax) }
- estVide: function estVide()
- faitPlein: function faitPlein(essenceAjoute)
- rouler: function rouler(kilometres, conso)
- <prototype>: Object { … }
```

Bien que cela renverra un objet vide dans la console NodeJS, cet objet **existe.** Il est possible de les voir en utilisant:
```js
Object.getOwnPropertyNames(...)
```

> [!Success]+ Remarque sur un détail entre class et instance
> Les prototypes d'une classe par rapport à son instance sont totalements différents:
> ```js
> > Object.getOwnPropertyNames(new Voiture())
 >[ 'model', 'kilometrage', 'reservoir' ]
> > Object.getOwnPropertyNames(Voiture)
 >[ 'length', 'name', 'prototype' ]
> ```
> Cela s'explique tout simplement par le fait que les class ne sont pas instanciées, donc les méthodes utiles pour une instance ne le sont pas pour la classe en elle-même.
> Malgré tout, si la classe `Voiture` avait des propriétés et/ou méthodes **static**, elles seraient affichées avec `Object.getOwnPropertyNames(Voiture)` mais pas avec une instance.
> On peut également accéder à la class d'une instance en faisant `ma_voiture.constructor`.

## Itérateurs

On a déjà vu comment déclarer des boucles ([[content/S3/Développement Web/NodeJS/Basiques#Boucles]]), mais prenons le cas suivant:

Nous devons filtrer les étudiant qui ne passeront pas l'année, trier les étudiants par nom et renvoyer la liste des noms + prénoms.

Il suffira de faire:
```js
const nomsEtPrenoms = etudiants
	.filter(etu => etu.semestreOK())
	.sort((a, b) => a.nom.localeCompare(b.nom))
	.map(etu => `${etu.prenom} ${etu.nom}`);
```

L'avantage des iterateurs est qu'on peut les placer à la chaîne et ainsi manipuler un Array plus facilement que avec des boucles à répétition.