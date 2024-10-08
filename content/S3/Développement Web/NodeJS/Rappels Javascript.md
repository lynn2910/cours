---
title: Basiques
draft: false
tags: []
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
`x` est une variable **globale** (var), ça signifie que, une fois définie, `x` sera accessible de partout. Cela pourra poser problème, notamment si une variable avec le même nom est déclaré plus tard, c'est le concept de shadowing.

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


### Arguments de fonction

Il existe plusieurs façons de déclarer des arguments*

**La méthode classique:**
```js
function rouler(voiture){
	// ...
}
```

**Argument optionnel:**
```js
function rouler(voiture, type_route = "ville"){
	// ...
}

// On peut alors faire:
rouler(voiture);
rouler(voiture, "campagne");
rouler(voiture, type_route = "campagne");
```

**Arguments groupés:**
```js
function envoyer_logs(...logs){
	console.log(`${logs.length} logs ont été envoyés`);
}

// permet de capter tout les arguments après:
envoyer_logs("Hello", "World", 8, {}, [1, 2, 3]);
// On verra "5 logs ont été envoyés"
```

Pour plus de détails: [mdn Paramètres des fonctions](https://developer.mozilla.org/fr/docs/Web/JavaScript/Guide/Functions#param%C3%A8tres_des_fonctions)
## Manipulation de string

On peut manipuler un string de cette manière:
```js
console.log(`Le carré de ${x} est ${carre(x)}`);
```

Il suffit de placer son texte entre ``` `...` ```, il est alors possible d'appeler des fonctions, variables, [[#Conditions ternaires]] etc...

## Conditions

Les conditions sont très simples, elles s'expriment de cette manière:
```js
if (1 + 1 == 0) {
	console.log("Aucun soucis");
} else {
	console.log("...")
}
```

Il est également possible de retirer les accolades si on a une seule ligne.

#### Opérateurs de conditions

Il y a différents opérateurs de conditions:
- `==` permet de tester une égalité sans vérifier le type (`1 == "1"` sera vrai)
- `===` est identique mais compare le type, `1 === 1` sera faux
- `!` est la négation, il peut être placé avant `=` et `==` (`!=` et `!==`)
- On retrouve les opérateurs `>`, `<`, `>=` et `<=` comme en Java

> [!Warning] Attention au `=`
> Mettre un `=` dans une condition ne va pas provoquer d'erreurs (oui oui), par exemple: (exemple testé dans la console, tapez `node` dans le terminal pour tester vous-mêmes)
> ```js
> > let x = 2
undefined
> > if (x = 5){
> ... console.log("x == 5");
> ... }
> x == 5
> undefined
> > console.log(x)
> 5
> undefined
> ```
> Heureusement, les IDE JetBrains sauront vous le faire remarquer.
### Conditions ternaires

On a vu les conditions avec des gros `if`, mais parfois c'est gênant. Par exemple, prenons l'exemple où on afficher "majeur" ou "mineur" selon la variable `age`.

Au lieu de prendre plusieurs lignes et une variable, on peut faire ceci:
```js
const age = 19;
console.log(`Cet élève a ${age >= 18 ? "majeur" : "mineur"}`);
```

On utilise la syntaxe `condition ? vrai : faux`, l'avantage est qu'elle peut être placée n'importe où: déclaration de variable, console.log, argument de fonction..., plus utile pour des petites conditions donc.

### Conditions en série

Pour exprimer une condition en série, on peut utiliser l'instruction `switch`:
```js
let language = "fr";

switch (language){
	case "fr": {
		console.log("Francais");
		break; // permet de ne pas exécuter les instructions suivantes
	}
	case "en-US": // Pas de break => si en-UK ou en-US est vrai, alors on verra "English"
	case "en-UK": {
		console.log("English");
		break;
	}
	default:
		console.log(`Langue inconnue: ${language}`);
}
```

## Boucles 

Les boucles similaires à Java
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

console.log(`Il y a ${noms.length} enregistrés.`);

let i = 0;
noms.forEach(function(nom){
	if (nom.toUpperCase().startsWith("A"))
		i++;
});

console.log(`${i} noms commencent par la lettre "A"`)
```

#### Boucles sur un Array

Il est possible d'utiliser plusieurs boucles:
```js
for (let nom of noms){
	console.log(nom); // Affichera tout les noms dans l'ordre où ils sont dans l'Array
}
```
```js
for (let nom in noms){
	console.log(nom); // Ce ne sera pas le nom mais l'index.
}
```
```js
// rien de nouveau à ce niveau, c'est comme en Java.
for (let i = 0; i <= 5; i++) {
	console.log(i);
}
```

On peut également opter pour des méthodes tel que `.forEach(fonction)` et `.map(fonction)`


#### Déstructurer un Array

On peut déstructurer un Array de cette façon:
```js
let arr1 = [1, 2, 3];
let arr2 = [4, 5, 6];

console.log([...arr1, ...arr2]);
```
On obtiendrais alors:
```js
[ 1, 2, 3, 4, 5, 6 ]
```

> [!Success] Remarque
> Cela peut marcher avec PLEINS de types (object, Map, ...)
### Object

> [!Warning] Attention
> A ne pas confondre avec le terme `Object` dans la POO! Un `Object` en **POO** n'est pas *l'object* qu'on va voir maintenant.


Un object en javascript est similaire à un **dictionnaire**, genre LITTERALEMENT.

On déclare un object de cette manière:
```js
let cat = {
	name: "Plume",
	age: 9,
	owner: "Cédric",
	toys: ["Ficelle", "Main"],
	pet: function(){
		console.log(`${this.name} ronronne`);
	}
}
```

On peut ensuite y accéder de cette manière:
```js
console.log(`Mon chat s'appelle ${cat.name}`);
```

L'avantage d'un object est qu'il permet de structurer les données de nombreuses manières mais surtout, il est TRES efficace.

> [!Summary]+ Raison de son efficacité
> Les object est ce qu'on peut appeler des `HashMap`. Un `Map` est une structure de donnée clé-valeur où chaque valeur est associée à une clé. Un object va utiliser ce principe.
> 
> Son efficacité vient du fait qu'il est très efficace d'associer une clé à une référence mémoire. Il n'est pas nécessaire de stocker les données dans un Array qu'il faudra parcourir.
> Quand on accède à une clé, on va directement recevoir son adresse mémoire, ce qu'il le rend très rapide.
> 
> La structure de donnée `Map` (voir [[POO]]) est similaire à l'exception que la clé peut être de tout type (Object, array, instance de class, ...) et possède des méthodes plus "haut-niveau" (.get, .set, ...) alors qu'un object est plus "primitif" et ses clés seront en général des chaines de caractères et nombres.

#### Méthodes utiles

Il existe plusieurs méthodes utiles à connaitre.

```js
const properties = Object.keys(cat); // permet de récupérer un Array avec les clés
console.log(`La variable 'cat' possède les propriétés suivantes: ${properties}");
```
```js
Object.values(cat); // permet d'obtenir toutes les valeurs
```
```js
Object.entries(cat); // Renvoit les paires clé-valeur
// On retrouvera par exemple ceci:
[
  [ 'name', 'Plume' ],
  [ 'age', 9 ],
  [ 'owner', 'Cédric' ],
  [ 'toys', [ 'Ficelle', 'Main' ] ],
  [ 'pet', [Function: pet] ]
]
```

Il existe de nombreuses autres méthodes intéressantes détaillées sur [mdn](https://developer.mozilla.org/fr/docs/Web/JavaScript/Reference/Global_Objects/Object)