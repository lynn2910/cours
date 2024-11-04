---
title: Introduction à tailwind
draft: false
tags:
---
Dans cette page, nous allons voir toutes les fonctionnalités principales de tailwind à travers un petit projet.

Nous allons tenter de reproduire cette barre de recherche de Google Drive :
![[Pasted image 20241104191623.png]]

Pour ce faire, nous allons utiliser la couleur `gray-900` (`#111827`) en guise de fond et `gray-800` (`#1f2937`) pour la couleur dans les boutons.

> [!info] Remarques
> Afin de simplifier l'exemple, on n'ajoutera pas les icônes pour chaque bouton, uniquement pour la barre de recherche
> 
> Seul les classes pertinentes seront affichés par soucis de lisibilité
## Layout de base

Pour le layout HTML, on ne va pas réinventer la roue :
```html
<div>
  <h1>Bienvenue dans Drive</h1>
  
  <!-- Barre de recherche -->
  <div>
    <svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24">
      <path d="M10 18a7.952 7.952 0 0 0 4.897-1.688l4.396 4.396 1.414-1.414-4.396-4.396A7.952 7.952 0 0 0 18 10c0-4.411-3.589-8-8-8s-8 3.589-8 8 3.589 8 8 8zm0-14c3.309 0 6 2.691 6 6s-2.691 6-6 6-6-2.691-6-6 2.691-6 6-6z"></path>
    </svg>
    <p>Rechercher dans Drive</p>
  </div>
  
  <!-- Boutons -->
  <div>
    <div>
      <p>Type</p>
    </div>

    <div>
      <p>Contacts</p>
    </div>

    <div>
      <p>Date de modification</p>
    </div>

    <div>
      <p>Emplacement</p>
    </div>
  </div>
</div>
```

On a alors quelque chose de pas très... esthétique:
![[Pasted image 20241104192448.png]]

---
## Styliser le fond

On va utiliser les propriétés suivantes :
- `bg-<color>`
- `text-<color>`

```html
<div class="bg-gray-900 text-gray-400 mx-auto">
  <h1>Bienvenue dans Drive</h1>
  ...
</div>
```

On a désormais :
![[Pasted image 20241104192803.png]]
---
## Centrer tout ça

On va désormais faire appel au [flex](https://developer.mozilla.org/fr/docs/Web/CSS/flex) et à la margin/padding afin de centrer et mettre de l'ordre là-dedans.

```html
<div class="... flex align-middle content-center flex-col">
  <h1>Bienvenue dans Drive</h1>
  ...
</div>
```

Ça donne ceci :
![[Pasted image 20241104193609.png]]

Ce qui déjà NETTEMENT mieux, mais on va encore jouer avec le `flex`.

---

## Titre "Bienvenue dans Drive"

On va utiliser `text-<size>` et `font-<weight>` pour le gras.

```html
  <h1 class="text-3xl font-extrabold mx-auto">Bienvenue dans Drive</h1>
```

> [!tip] Pourquoi `mx-auto`?
> Une propriété incroyable des margin en CSS est que, en la définissant en tant que `auto`, la margin va alors tenter d'être maximisée. Ce qui se passe quand la margin est définie à auto sur l'axe horizontal ou vertical (voir les deux!), chaque coté va être maximisé.
> 
> On aura alors une situation où 

Il existe également des classes telles que :
- `font-bold`
- `font-black`
- `text-lm`
- `text-xl`
- Et encore plus (*voir [font size - tailwindcss.com](https://tailwindcss.com/docs/font-size) et [font weight - tailwindcss.com](https://tailwindcss.com/docs/font-weight)*)

Nous avons maintenant :
![[Pasted image 20241104193959.png]]

---

## Barre de recherche

On va découvrir quelques petites propriétés dans cette partie.

Voici le HTML avec les classes tailwind:
```html
  <div class="flex flex-row align-middle bg-gray-800 rounded-3xl py-2 px-3 my-5 w-[800px]">
    <svg class="fill-gray-400 my-auto mr-4" width="20" height="20" ...>
		...
    </svg>
    <p>Rechercher dans Drive</p>
  </div>
```

Et le résultat :
![[Pasted image 20241104195117.png]]

Dans cette partie, on retrouve le padding (`py-2 px-3`).

Avec tailwind:
- horizontal : `x`
- vertical : `y`
- droite : `r`
- gauche : `l`
- haut : `t`
- bas : `b`

De ce fait, `py-2` applique un padding de taille `2` (`0.5rem` de mémoire) sur l'axe vertical.

Sur le `svg`, on va également retrouver `my-auto mr-4`. On applique un margin vertical automatique pour centrer (comme on a vu la technique), et `mr-4` ajoute un margin de taille `2` à droite.

La classe `rounded-<taille>` (ou sans taille) permet quant à elle de rondir les bords de notre div. 

Et pour finir, sans doute le plus "complexe" de tout ce mini projet, c'est `w-[800px]`.
Cette classe permet de définir une `width`, mais la syntaxe `[<valeur>]` permet de définir une taille qui n'est pas de base dans tailwind. On peut également y mettre des pourcentages, tel que `[50%]`.

On peut alors utiliser des classes telles que `mr-[10%]`, etc.

---
## Les boutons

Comme vu précédemment, on centre nos boutons :
```html
<div class="flex flew-row align-middle content-center mx-auto">
	<!-- Nos boutons -->
</div>
```

Notre bouton typique aura alors ce style :
```html
<div class="py-2 px-5 mx-2 rounded-3xl bg-gray-800 flex flex-row align-middle cursor-pointer hover:bg-gray-700">
  <p>Type</p>
</div>
```

Rien de nouveau, sauf `hover:bg-gray-700`.

`hover:<style>` permet d'appliquer un style quand on passe dessus.

> [!tip]- La classe `cursor-pointer`
> Cette classe permet au curseur d'apparaitre comme s'il y avait une interaction ; Utile pour signaler à l'utilisateur qu'il peut interagir avec un élément.

Voici le résultat:
![[Pasted image 20241104203258.png]]

## Conclusion

Voici le résultat final :
![[Pasted image 20241104203316.png]]
> [!jsp]- Comparer avec le modèle
> ![[Pasted image 20241104191623.png]]

Comme on peut voir entre les deux, c'est facile de pouvoir styliser des éléments avec tailwind.

---

**Conseil**

Maintenant que les bases sont posées, il est recommandé d'expérimenter avec.

Exemples d'interfaces faciles pour s'entrainer :
- La page d'accueil du moteur de recherche Google : [google.com](https://google.com)
- Une page de contact (un formulaire avec nom, prénom, mail, etc.)
- Une page où présenter une recette de cuisine
- Un calculateur d'âge (on rentre la date et ça nous affiche le nombre d'années, de mois et de jours, évidemment stylisé).
- Un générateur de mot de passe.

---

Pour pouvoir le voir sur codepen avec le code et le modifier : [codepen.com](https://codepen.io/lynn2910/pen/JjgBwrZ)

Et voici le code final (c'est *littéralement* un copié collé du code dans codepen):
```html
<div class="bg-gray-900 text-gray-400 h-[50%] my-auto flex align-middle content-center flex-col">
  <h1 class="text-3xl font-extrabold mx-auto">Bienvenue dans Drive</h1>
  
  <!-- Barre de recherche -->
  <div class="flex flex-row align-middle bg-gray-800 rounded-3xl py-2 px-3 my-5 w-[800px]">
    <svg class="fill-gray-400 my-auto mr-4" xmlns="http://www.w3.org/2000/svg" width="20" height="20" viewBox="0 0 24 24">
      <path d="M10 18a7.952 7.952 0 0 0 4.897-1.688l4.396 4.396 1.414-1.414-4.396-4.396A7.952 7.952 0 0 0 18 10c0-4.411-3.589-8-8-8s-8 3.589-8 8 3.589 8 8 8zm0-14c3.309 0 6 2.691 6 6s-2.691 6-6 6-6-2.691-6-6 2.691-6 6-6z"></path>
    </svg>
    <p>Rechercher dans Drive</p>
  </div>
  
  <!-- Boutons -->
  <div class="flex flew-row align-middle content-center mx-auto">
    <div class="py-2 px-5 mx-2 rounded-3xl bg-gray-800 flex flex-row align-middle cursor-pointer hover:bg-gray-700">
      <p>Type</p>
    </div>

    <div class="py-2 px-5 mx-2 rounded-3xl bg-gray-800 flex flex-row align-middle cursor-pointer hover:bg-gray-700">
      <p>Contacts</p>
    </div>

    <div class="py-2 px-5 mx-2 rounded-3xl bg-gray-800 flex flex-row align-middle cursor-pointer hover:bg-gray-700">
      <p>Date de modification</p>
    </div>
    
    <div class="py-2 px-5 mx-2 rounded-3xl bg-gray-800 flex flex-row align-middle cursor-pointer hover:bg-gray-700">
      <p>Emplacement</p>
    </div>
  </div>
</div>
```

> [!tip] Note sur le code
> Dans le code, on peut remarquer les boutons sont littéralement des copiés-collés
> On va bien souvent préférer, si on utilise un framework tel que VueJS, créer un composant (voir [[VueJS]])
> 
> Avec **Tailwind**, on ne copie pas deux fois la même suite de classe, on va préférer créer des composants. Dans ce cas, par exemple, on va créer un composant `SearchButton` avec en `props` le nom. 

