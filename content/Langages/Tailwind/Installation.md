---
title: Installation
draft: false
tags:
---

## HTML vanilla

> [!quote] Par `vanilla` est entendu tout projet qui n'utilise pas un framework, donc que du bon vieux HTML

Installer les dépendances :
```shell
npm install -D tailwindcss@latest postcss@latest autoprefixer@latest
```

Ensuite, on va ajouter les fichiers `tailwind.config.js` et `postcss.config.js` avec la commande puissante :
```shell
npx tailwindcss init -p
```

On va ensuite créer notre fichier `input.css` avec ces trois directives :
```css
@tailwind base;
@tailwind components;
@tailwind utilities;
```

En avant-dernière étape, on doit lancer `tailwindcss` afin qu'il puisse détecter les changements dans notre projet et automatiquement ajouté les class dans un autre fichier :
```shell
npx tailwindcss -i ./src/input.css -o ./src/output.css --watch
```

> [!warning] Note sur le CLI de tailwind
> Avant de râler que tailwind ne marche plus quand vous lancez votre projet, vérifiez que le cli est lancé. Il est très facile d'oublier de lancer cette commande.

Avec la commande au-dessus, on aura alors un fichier `output.css` qui apparaitra. Il suffit alors d'ajouter cette ligne dans notre HTML :
```css
<link href="./output.css" rel="stylesheet">
```

## VueJS

> [!success] Cette méthode fonctionne avec Vue2 et Vue3

Installer les dépendances :
```shell
npm install -D tailwindcss@latest postcss@latest autoprefixer@latest
```

Ensuite, on va ajouter les fichiers `tailwind.config.js` et `postcss.config.js` avec la commande puissante :
```shell
npx tailwindcss init -p
```

Si le fichier `postcss.config.js` n'est pas intéressant dans notre cas (sauf dans des cas précis, mais les personnes qui y touchent sont déjà des développeurs avancés).


Le fichier `tailwind.config.js` ressemble à ceci :
```js
module.exports = {
  purge: [],
  darkMode: 'media',
  theme: {
    extend: {},
  },
  variants: {
    extend: {},
  },
  plugins: [],
}
```

Mais, dans son état actuel, tailwind n'est pas encore prêt, il va falloir changer le champ `purge`

On va alors remplacer cette ligne par :
```diff
  module.exports = {
-   purge: [],
+   purge: ['./index.html', './public/index.html', './src/**/*.{vue,js,ts,jsx,tsx}'],
    darkMode: false, // or 'media' or 'class'
    theme: {
      extend: {},
    },
    variants: {
      extend: {},
    },
    plugins: [],
  }
```

On va finalement créer un fichier `index.css` dans le dossier `src` et utiliser trois directives de tailwind (`base`, `components`, `utilities`) afin que tailwind sache où écrire les classes :

```css
@tailwind base;
@tailwind components;
@tailwind utilities;
```

Finalement, il suffit d'ajouter dans le fichier `./src/main.js` (ou `main.ts`):
```js
import './index.css'
```

Et c'est tout. Tailwind peut désormais être utilisé sur le projet.