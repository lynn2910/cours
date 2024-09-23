---
title: Directives
draft: false
tags:
---
Les démonstrations se basent sur [ce code source](https://cours-info.iut-bm.univ-fcomte.fr/upload/supports/S3/Vuejs/TD/td2/vuejs-td2-src.tgz)

> [!tip]- Tester le code source
> - Copier le fichier TD2Demo1.vue dans TD2Demo.vue
> - lancer npm run serve puis visualiser le résultat dans le navigateur (par ex. [http://localhost:8080](http://localhost:8080))
> - Montrer le code de App.vue. On constate que la valeur de la variable title est bien affiché comme un titre.
> - le composant TD2Demo1 importé dans App.vue s'utilise simplement en écrivant une balise \<TD2Demo1>.
> - Idem pour les variables msg et htmlText définies dans TD2Demo1.
> - Comme la deuxième contient du HTML, ce dernier n'est effectivement pas interprété avec {{ }}.
> - On remarque qu'avec v-html, le texte interne à la balise \<span> n'est pas affiché. Seul le contenu de htmlText est interprété. C'est le même comportement quelle que soit la balise.
> - Modifier le contenu de msg => le texte affiché change
## Afficher du texte dynamique

Pour afficher du texte dynamiquement (dans des balises tel que `<p>`, `<span>`, `<li>`, ...) on utiliser cette syntaxe:

```vue
<p>{{ msg }}</p>
```

**Exemple:**
```vue
<template>
	<p>{{ msg }}</p>
</template>

<script>
export default {
  name: 'Exemple',
  data: () => {
    return {
		msg: "Hello world!"
    }
  }
}
</script>
```

La valeur entre `{{ }}` permet d'y intégrer une expression JavaScript. On est donc libre d'utiliser un nom de variable, un appel à une fonction etc...

> [!warning]+ Limitations
> On ne peut pas utiliser les mot-clés de contrôle (if, for, while, ...).
> Pour intégrer une condition ternaire, voir [[Basiques#Conditions ternaires]]
> Les opérateurs logiques et arithmétiques sont en revanche utilisables.

## Intégrer du HTML

La syntaxe `{{ }}` ne permet pas d'intégrer du HTML. Ce dernier sera affiché tel quel. La solution est donc d'utiliser `v-html` de cette manière:

```vue
<template>
	<span v-html="htmlExemple"></span>
</template>

<script>
export default {
	name: "Exemple2",
	data: () => {
		return {
			"htmlExemple": "<strong>Hello</strong> world!"
		}
	}
}
</script>
```

> [!danger] Failles de sécurité
> Ne **JAMAIS** utiliser `v-html` avec une variable entrée ou pouvant être modifiée par l'utilisateur. Cela permettrait d'injecter du code Javascript

## v-bind

Cette directive permet de dire à VueJS de placer la variable dans le DOM.

Si les `{{ }}` (voir [[#Afficher du texte dynamique]]) peuvent le faire, elles ne permettent pas de définir, par exemple, la valeur dans une balise `<input>`.

On utilisera alors ces deux syntaxes:
```vue
<input       :value="valeurInput">

<input v-bind:value="valeurImport">
```

De la même façon que le texte dynamique, on peut utiliser des expressions JavaScript.

On peut également définir des attributs tel que `readonly`, les class mais aussi le style.

**Exemple:**
```vue
<template>
	<h1 :style="textStyle">Prix du produit</h1>
	<input
		type="number"
		:readonly="readonly"
		:value="price.toFixed(2)"
		:class="price < 10 ? ['price-low'] : []">
</template>

<script>
	export default {
		name: "Exemple3",
		data: () => {
			return {
				price: 20,
				readonly: false,
				textStyle: {
					color: "gray",
					fontSize: "30px"
				}
			}
		}
	}
</script>
```


> [!warning] TODO
> Je dois rédiger la suite


2.3°/ relation bidirectionnelle : v-model

- Si la valeur du champ de saisie change, il serait pratique que la valeur du champ de data qui sert à alimenter le champ de saisie change en retour. Cette relation bidirectionnelle peut être mise ne place grâce à la directive v-model.
- Cette directive s'applique aux différents type de balise <input>. On peut donc l'utiliser sur des champs de saisie mais aussi des boutons radio et cases à cocher.
- On peut également l'utiliser avec la balise \<select>

_Démonstration :_

- Copier le fichier TD2Demo3.vue dans TD2Demo.vue
- lancer npm run serve puis visualiser le résultat dans le navigateur (par ex. [http://localhost:8080](http://localhost:8080))
- Montrer le code de TD2Demo3.
- on constate que le bouton radio n°0 est bien sélectionné => relation type v-bind de selectedItem vers <input> fonctionne toujours.
- cliquer sur un autre bouton radio : le nom de l'item sélectionné change => relation de <input> vers selectedItem fonctionne également.
- sélectionner un prix d'item => la liste des prix change, signe que le champ de data a bien été modifié en retour.

2.4°/ affichage de "listes" : v-for

- Dans la démonstration précédente, on remarque que le code devient très vite redondant dès lors que l'on manipule des éléments d'un tableau.
- Fort heureusement, vuejs propose la directive v-for permettant d'itérer sur un tableau ou sur les champs d'un objet pour instancier plusieurs fois une même balise, mais avec des valeurs d'attribut différentes.
- Grâce à la balise \<div> sur laquelle on utilise v-for, on peut même répéter plusieurs fois plusieurs balises.
- La syntaxe v-for est multiple, selon qu'on l'applique à un tableau, un objet, que l'on désire récupérer les indices dans le tableau, les noms de champs dans l'objet. Quelques exemples :
    - v-for = "el in tab" : el prendra successivement comme valeur chaque élément de tab
    - v-for = "(el, idx) in tab" : idem et idx vaudra 0, 1, ...
    - v-for = "(val, name, idx) in obj" : val prendra successivement comme valeur chaque valeur de champ dans obj, name sera le nom de champ et idx vaudra 0,1, ...
- Dans ces exemples, le nom des itérateurs est au choix du développeur. On peut donc remplacer el, idx, val, name par ce que l'on veut, du moment qu'on l'utilise dans le reste de la (ou les) balise(s) utilisant v-for.

- IMPORTANT : lorsque l'on utilise la configuration par défaut créée par vue-cli, l'utilisation de v-for sur une balise implique de donner une valeur à son attribut key. Comme il faut que cette valeur soit différente pour chaque balise créée avec v-for, on utilise très souvent l'index comme valeur (cf. démonstration).

_Démonstration :_

- Copier le fichier TD2Demo4.vue dans TD2Demo.vue
- lancer npm run serve puis visualiser le résultat dans le navigateur (par ex. [http://localhost:8080](http://localhost:8080))
- Montrer le code de TD2Demo4.
- Cet exemple fait la même chose que le précédent, avec en plus, le parcours des champs de l'item sélectionné.
- On remarque que pour le v-for du \<div> qui affiche la liste des items, on a pas besoin de fixer l'attribut key pour les balises répétées mais juste dans \<div>.
- On remarque également que les attributs id et for utilisés dans ce \<div> sont cette fois affectés grâce à v-bind. Normal car leur valeur doit être différente pour chaque entrée de la liste à puce. donc elle est calculée grâce à une expression javascript.
- Enfin, même si on ne gagne pas beaucoup de lignes de code pour cet exemple, on imagine aisément le gain lorsque le tableau que l'on doit parcourir a plusieurs dizaine/centaines d'éléments. Qui plus est, v-for fonctionne toujours, même si la taille du tableau évolue !

_Remarques :_

- on peut imbriquer des v-for. C'est particulièrement utile lorsque l'on veut générer une balise \<table>, par ex, à partir un tableau dont les éléments contiennent eux-mêmes des tableaux
- pour nue même balise, on peut utiliser conjointement v-for et v-bind, par exemple pour passer des valeurs d'attributs différentes selon l'instance de la balise.
- v-for s'applique à toute balise HTML mais aussi celles des composants.

2.5°/ affichage conditionnel : v-if, v-else-if, v-else  

- Grâce à v-bind, on peut générer des changements de valeur/style conditionnels.
- En revanche, cela ne permet pas de carrément changer de balise de façon conditionnelle.
- Pour cela, il existe les directives v-if, v-else-if, qui prennent en paramètre une expression JS (comme les moustache et v-bind), et v-else, qui n'a pas de paramètre.
- Ces directives permettent de créer dans le DOM un ou plusieurs éléments en fonctions de l'évaluation true ou false de l'expression.
- Attention ! ces directives ne s'appliquent qu'à la balise où elles apparaissent. Cela implique que sauf exception, le rendu conditionnel ne fonctionnera pas sur des balises imbriquées.
- Parmi les exceptions : balises \<div> et \<template>.

_Démonstration :_

- Copier le fichier TD2Demo5.vue dans TD2Demo.vue
- lancer npm run serve puis visualiser le résultat dans le navigateur (par ex. [http://localhost:8080](http://localhost:8080))
- Montrer le code de TD2Demo5.
- La première partie de la page illustre le fait que les balises utilisé dans v-if, v-else-if et v-else peuvent être différentes.
- La seconde partie illustre l'utilisation de v-if sur \<div>. C'est le seul moyen (avec template) de rendre conditionnel l'affichage de tout un ensemble de balises. Bien entendu, celles-ci doivent être imbriquées dans \<div>