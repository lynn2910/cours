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
> Pour intégrer une condition ternaire, voir [[Rappels Javascript#Conditions ternaires]]
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

## Directives
### v-bind

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


### v-model: liaison bi-directionnelle

Jusqu'à présent, nous avons vu comment afficher des données depuis notre objet data dans le template à l'aide de l'interpolation de texte (`{{ }}`) et de la directive **v-bind**. Cependant, il est souvent nécessaire d'établir une relation plus interactive entre le template et les données : lorsque l'utilisateur modifie une valeur dans le template, cette modification doit être répercutée dans l'objet data.

**C'est là qu'intervient la directive `v-model`**. Elle permet de créer une liaison bidirectionnelle entre un élément du template (comme un champ de saisie, un bouton radio, une case à cocher ou une liste déroulante) et une propriété de l'objet data.

**Exemple**
```vue
<template>
  <input type="text" v-model="message">
  <p>Vous avez tapé : {{ message }}</p>
</template>

<script>
export default {
	name: "Exemple4",
	data() {
		return {
		    message: 'Hello, world!'
	    }
    }
}
</script>
```

> [!tip] Utilisation courante
> On retrouvera cette directive en général dans les **input**, **checkboxes**, **bouton radio** et **select**.

### v-for: afficher des 'listes'

Lorsqu'on souhaite afficher un ensemble d'éléments similaires, comme une liste d'articles, une série de cartes ou une table de données, il devient rapidement fastidieux de répéter manuellement chaque élément dans le template. **Vue.js** propose une solution élégante pour résoudre ce problème : la **directive `v-for`**.

`v-for` permet d'itérer sur un tableau ou un objet, et de générer dynamiquement du contenu en fonction des éléments de cette structure de données. Cela permet de créer des listes, des tableaux, et bien d'autres éléments réutilisables.

La syntaxe est la suivante:
```vue
<element v-for="(item, index) in items" :key="key"></element>
```

> [!warning] L'attribut `:key`
> Quand on utilise la directive **v-for**, il est **impératif** d'ajouter l'attribut `:key=".."` qui sera une valeur unique, par exemple l'index, ou l'identifiant d'un utilisateur.
> On évitera **à tout prix** l'index quand la liste peut-être réordonnée, trier, ou si des éléments peuvent être ajoutés/supprimés **au milieu** de la liste.
> 
> Plus d'informations: [stackoverflow](https://stackoverflow.com/questions/44531510/why-not-always-use-the-index-as-the-key-in-a-vue-js-for-loop)

On a plusieurs possibilités d'utilisations:

**Itérer sur un `Array`:**
```vue
<ul>
  <li v-for="(fruit, index) in fruits" :key="index">
    {{ index + 1 }}. {{ fruit }}
  </li>
</ul>
```

**Itérer sur un Object:**
```vue
<ul>
  <li v-for="(value, key, _index) in person" :key="key">
    {{ key }}: {{ value }}
  </li>
</ul>
```

> [!tip] Plusieurs remarques:
> 1. On peut imbriquer des **v-for**, c'est très utile pour les tables par exemple.
> 2. On peut utilise **v-for**, **v-bind** et **v-model** ensemble, par exemple avec des boutons radios.
> 3. **v-for** peut s'appliquer à tout balise HTML (`div`, `p`, `option`, ...) mais aussi à tout les composants VueJS (`ShopCard`, `PrestatairePresentation`, ...)

### Affichage conditionnel: v-if, v-else, v-else-if

Ces directives permettent de contrôler l'affichage d'éléments dans le DOM en fonction d'une condition booléenne.

- **`v-if`:** Affiche un élément si l'expression associée est évaluée à `true`.
- **`v-else-if`:** Doit suivre un `v-if` ou un autre `v-else-if`. Il est affiché si toutes les expressions précédentes sont fausses et que la sienne est vraie.
- **`v-else`:** Doit suivre un `v-if` ou un `v-else-if`. Il est affiché si toutes les expressions précédentes sont fausses.

**Exemple:**
```vue
<template>
	<!-- Première condition -->
	<div v-if="showParagraph">
	    <p>Ce paragraphe s'affiche si showParagraph est vrai.</p>
	</div>
	<div v-else-if="showOtherParagraph">
		<p>Ce paragraphe s'affiche si showParagraph est faux et showOtherParagraph est vrai.</p>
	</div>
	<div v-else>
		<p>Aucun des deux paragraphes ne s'affiche.</p>
	</div>
</template>

<script>
export default {
	name: "Exemple5",
	data() {
		return {
			showParagraph: true,
			showOtherParagraph: false
		}
	}
}
</script>
```

> [!warning] Porté de ces directives
> Ces 3 directives conditionnelles s'appliquent à l'élément (HTML ou composant) où elle est déclarée, et donc à tout les enfants de cet élément.
