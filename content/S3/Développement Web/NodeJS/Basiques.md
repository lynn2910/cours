---
title: Basiques
draft: false
tags:
  - Cours
---
## Déclaration de variable

Il existe 3 façons de déclarer une variable:
```js
let cat = "meow";
```
```js
const dog = "wouf"
```
```js
var snake = "ssssss"
```

La différence entre `const` et `let`:
- `const` ne permet pas de réécrire la variable, tel que `dog = "wouf wouf"` **ne marchera pas.**
- `let` permet de réécrire la variable.
- Dans le cas où c'est un Object (Class, Array, Object...), il est possible de les modifier grâce aux [références](https://developer.mozilla.org/fr/docs/Glossary/Object_reference)

Pour `var`, c'est un `let` sous stéroïdes (voir [[#Scope]])
## Scope

Le scope d'une variable consiste à ce que cette variable ne soit définie **que** dans son bloc de code, sans dépasser.

Ainsi, on peut avoir le code suivant:
```js
var x = 10;

// On créer un scope
{
	x += 10;
	console.log(x); // 20
}

console.log(x); // 20
```
`x` est une variable **globale** (var), ça signifie que, une fois définie, `x` sera accessible de partout. Cela pourra poser problème, notamment si une variable avec le même nom est déclaré plus tard, c'est le concept de .

Un autre soucis (ou avantage, mais c'est rare) est que `x` sera égale à `undefined` avant qu'il soit déclarer ([mdn var#Hoisting](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/var#hoisting))

Voir [mdn (var)](https://developer.mozilla.org/fr/docs/Web/JavaScript/Reference/Statements/var) pour plus de détails

> [!Warning] Avertissement
> Il est généralement recommandé d'éviter `var` comme la peste, sauf dans des cas biens précis où le développeur sais ce qu'il fait et le documente.

## Fonctions

Il existe plusieurs façons de déclarer une fonction:

### 1. Fonctions anonymes

Une fonction anonyme peut se déclarer de deux façons:
```js
(x) => x ** 2
```
```js
function(x){ return x ** 2; }
```

En général, les fonctions fléchées sont utilisées pour des petites opérations, tel que:
```js
const note = notes.find((title) => title === args.title);
```
Il n'est également pas nécessaire de préciser l'instruction `return`

Au contraire, la seconde syntaxe sera en générale utilisée quand le code est plus complexe qu'une simple expression.

> [!success]+ Syntaxe Bonus
> Il est aussi possible d'exprimer une fonction fléchée de cette manière:
> ```js
> (a) => {
> 	if (a % 26 == 0) {
> 		console.log("'a' est un multiple de 26");
> 	} else {
> 		console.log("'a' n'est pas multiple de 26");
> 	}
> }
> ```
> Cette version est exactement la même que `function(..){..}`, c'est une préférence personnelle

### 2. Fonctions nommées

Il n'existe qu'une façon de déclarer une fonction nommée:
```js
function carre(x){
	return x ** 2;
}
```

Il est aussi tout à fait possible d'utiliser une fonction anonyme et de l'assigner à une fonction.

## Manipulation de string

On peut manipuler un string de cette manière:
```js
console.log(`Le carré de ${x} est ${carre(x)}`);
```

Il suffit de placer son texte entre ``` `...` ```, il est alors possible d'appeler des fonctions, variables, [[#Conditions ternaires]] etc...

## Conditions

TODO
### Conditions ternaires

TODO

## Types

On retrouvera:
- `Number`: Littéralement un nombre, entier ou décimale
- `String`: Une chaîne de caractères

Mais on a aussi quelques types supplémentaires

### Array

Un `Array` est l'équivalent d'une liste en python.

On peut déclarer un array de plusieurs façons:
```js
const arr1 = [];
const arr2 = new Array();
```

Pour la seconde méthode, voir [[POO]]

Voici un exemple:
```js
const noms = ["Leslie", "Marvyn", "Baptiste"];

noms.push("Anna"); // On ajoute un nom dans l'Array

console.log("Il y a )

```