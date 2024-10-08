---
title: NodeJS & ecosystème
draft: false
tags:
---
L'écosystème de NodeJS est à la fois une mine d'or et la maison hantée du village. On va voir les outils principaux

## NodeJS

NodeJS est le runtime, il permet d'exécuter un script JS.

## Gestionnaires de paquets

### npm, le plus connus

`npm`, pour **N**ode **P**ackage **M**anager, est l'outil qui va permettre d'installer des dépendances et créer des projets, du plus simple au plus complexe.

Pour ce faire, il va créer un fichier appelé `package.json`.

Exemple:
```json
{
  "name": "my-project",
  "version": "1.0.0",
  "description": "An example to show the package.json",
  "main": "src/main.js",
  "author": "Jon Doe jon@example.com <https://www.example.com>",
  "licence": "ISC",
  "private": true,
  "bugs": "<https://github.com/whatever/package/issues>",
  "scripts": {
    "dev": "webpack-dev-server --inline --progress --config build/webpack.dev.conf.js",
    "start": "npm run dev",
    "unit": "jest --config test/unit/jest.conf.js --coverage",
    "test": "npm run unit",
    "lint": "eslint --ext .js,.vue src test/unit",
    "build": "node build/build.js"
  },
  "dependencies": {
    "vue": "^2.5.2"
  },
  "devDependencies": {
    "autoprefixer": "^7.1.2",
    "babel-core": "^6.22.1",
    "babel-eslint": "^8.2.1",
    "babel-helper-vue-jsx-merge-props": "^2.0.3",
    "babel-jest": "^21.0.2",
    "babel-loader": "^7.1.1",
    "babel-plugin-dynamic-import-node": "^1.2.0",
    "babel-plugin-syntax-jsx": "^6.18.0",
    "babel-plugin-transform-es2015-modules-commonjs": "^6.26.0",
    "babel-plugin-transform-runtime": "^6.22.0",
    "babel-plugin-transform-vue-jsx": "^3.5.0",
    "babel-preset-env": "^1.3.2",
    "eslint": "^4.15.0",
    "file-loader": "^1.1.4",
    "jest": "^22.0.4",
    "vue-jest": "^1.0.2",
    "vue-loader": "^13.3.0",
    "vue-style-loader": "^3.0.1",
    "vue-template-compiler": "^2.5.2",
    "webpack": "^3.6.0",
    "webpack-bundle-analyzer": "^2.9.0",
    "webpack-dev-server": "^2.9.1",
    "webpack-merge": "^4.1.0"
  },
  "engines": {
    "node": ">= 6.0.0",
    "npm": ">= 3.0.0"
  }
}
```
Comme ça, ça parait barbare, mais en réalité c'est très utile.
Il est possible d'ajouter des dépendances (et dépendances PENDANT le développement, les `devDependencies`), de préciser un nom, une description, une version, un auteur, un github etc... 

On peut également définir des scripts.

Toutes les dépendances seront installées dans le dossier `node_modules`, mais attention, car ce dossier a tendance à très vite atteindre 100+ Mb en taille.

Référence: [npm](https://docs.npmjs.com/cli/v10/configuring-npm/package-json)

### yarn

Yarn est littéralement **npm**, mais il est connu pour être plus rapide, mais il a un autre avantage:

Il va installer toutes les dépendances dans un seul dossier et créer des liens symboliques, ce qui peut être très utile quand on a beaucoup de projets utilisant NodeJS

## Structure du code

Il n'y a aucune norme spécifique pour JavaScript, mais la structure suivante est recommandée:
```bash
├── src
|   ├── app.js
|   └── notes.js
├── package.json
└── node_modules
```

le dossier `src` est bien souvent utilisé pour y placer tout le code.

## Librairies et outils utiles

Il est recommandé, pour des **gros** projets:
- [eslint](https://eslint.org/): permet d'éviter de nombreuses erreurs de style de code qu'on peut faire. Compatible avec la suite JetBrains.
- [typescript](https://www.typescriptlang.org/): permet d'ajouter un système de typage en Javascript. Très utile pour renforcer la stabilité du projet.

## JavaScript Documentation (JSdoc)

La JSdoc est une façon de documenter le code, voici un exemple concret:
```js
/**
  * Greet someone with either a formal or non-formal sentence.
  * 
  * @param {string} name The name of the person who is greeted
  * @param {boolean} [formal] Whether we'll use the formal sentence
  * 
  * @return {string} The answer of the greeted person
  */
function greetPeople(name, formal = true){
  // ...
}
```

On peut voir que cette documentation, si elle parait beaucoup plus verbose et longue à taper, elle permet trois choses:
- On sais exactement quel paramètre sert à quoi et quel type est attendu
- On précise ce qui est renvoyé
- L'éditeur (surtout les IDE JetBrains) sont capable de comprendre la JSdoc et de proposer la bonne completion. (par exemple une erreur apparaitra si on donne un Array pour l'argument `name`)

Références:
- [JSdoc](https://jsdoc.app/)
- [JSdoc @param](https://jsdoc.app/tags-param)
- [JSdoc @return](https://jsdoc.app/tags-returns)

