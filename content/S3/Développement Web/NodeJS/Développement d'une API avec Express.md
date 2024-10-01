---
title: Développement d'une API avec Express
draft: false
tags:
---
## Mise en place

Pour démarrer un nouveau projet, il faut utiliser la commande `npm init`

Si le projet existe déjà (par exemple depuis github):
```bash
git clone <repo> && cd <repo>
```
puis
```bash
npm install
```

**npm** va alors installer toutes les dépendances

## ESM & CommonJS

### CommonJS

La syntaxe de base est:
```js
const { f1, f2, var1, var2 } = require("mon_fichier.js");
```

#### Utiliser ESM dans un projet commonjs

Pour le faire, on doit nommer le fichier `<nom>.mjs`

### ESM

La syntaxe de base est:
```js
import { f1, f2, var1, var2 } from "mon_fichier.js"
```

Pour utiliser **esm** dans un projet, il faut ajouter la propriété `"type": "module"` dans le fichier `package.json`
#### Utiliser CommonJS pour un projet ESM

Pour le faire, on doit nommer le fichier `<nom>.cjs`
### Différences

La différence principale est que, avec **commonjs**, nous pouvons importer des modules **au milieu** du projet.

> [!example]- Exemple
> ```js
> const fs = require("fs");
> function getNotes(){ /* ... */ }
> if (getNotes().length < 1) {
> 	const controller = require("mon_controller.js");
> 	controller();
> }
> ```

Alors que avec **esm**, ce sera impossible.

> [!tip] Format conseillé
> En général, on retrouver **ESM** (ou **es6**) sur les scripts JS exécuté côté client, alors que **CommonJS** est préféré par NodeJS
## Express

Une application express peut être crée de cette manière:
```js
const port = 3000;
const express = require("express");

const app = express();

app.get("/", function(req, res){
	res.send("Hello, world!")
})

app.listen(port, () => {
	console.log(`Listening on http://localhost:${port}`);
})
```

> [!tip]- Redémarrer automatiquement l'application
> On peut relancer l'application automatiquement avec [nodemon](https://www.npmjs.com/package/nodemon)
> Pour installer `nodemon`:
> ```bash
> npm install -g nodemon
> ```
> *`nodemon` sera alors disponible sur tout les projets*
> Puis, pour lancer nodemon:
> ```bash
> nodemon index.js
> ```
> On n'aura alors plus besoin de relancer l'application, elle se relancera automatiquement.


### Paramètres importantes

#### req.body

C'est la partie la plus substantielle de la requête. Elle contient les données que le client a envoyées au serveur dans le corps de la requête. C'est typiquement utilisé pour :

- **Envoyer des formulaires:** Lorsque un formulaire sur un site web est rempli, les données du formulaire sont généralement envoyées dans le corps de la requête.
- **Envoyer des données JSON:** Les API utilisent souvent le format JSON pour échanger des données. Ces données sont alors placées dans le corps de la requête.

> [!example]- Exemple
> Si un formulaire est envoyé, `req.body` contiendra les valeurs qui ont été saisies pour le nom, l'email, le mot de passe, etc.

#### req.params

Ce sont les valeurs dynamiques qui sont incluses directement dans l'URL de la requête. Elles sont généralement utilisées pour identifier des ressources spécifiques.

Voir [[#Requêtes avec paramètres]]

> [!example]- Exemple
> Dans l'URL `/users/123`, `123` est un paramètre. Il indique qu'on veut récupérer les informations de l'utilisateur ayant l'ID 123. `req.params` permettra d'accéder à cette valeur.

#### req.url

C'est l'URL complète de la requête, telle qu'elle est reçue par le serveur. Elle comprend le chemin, les paramètres de requête et parfois même les fragments.

> [!example]- Exemple
> Si la page `/products?category=electronics&page=2`, est demandée `req.url` contiendra cette chaîne complète, avec la query.
> Pour obtenir l'url SANS la query, il faudra utiliser un **regex**.
#### req.query

Ce sont les données supplémentaires qui sont ajoutées à l'URL après le point d'interrogation (?). Elles sont utilisées pour filtrer ou trier les résultats.

> [!example]- Exemple
> Dans l'URL `/products?category=electronics&page=2`, `category=electronics` et `page=2` sont des paramètres de requête. Ils indiquent qu'il faut afficher les produits de la catégorie "électronique" à partir de la deuxième page.

### Requêtes avec paramètres

Imaginons le scénario suivant: nous souhaitons créer une API qui va permettre d'obtenir des infos sur des fruits. On peut alors faire la requête `/fruits/:fruit`

On peut alors faire ceci:
```js
app.get('/fruits/:fruit', (req, res) => {
	const fruitID = req.params.fruit;
});
```

> [!danger] Limitations
> On ne mettra JAMAIS d'informations sensibles dans ces paramètres. On préfèrera utiliser le[[#req.body]]

### Tester les requêtes

Si les requêtes de type `GET` sont faciles à tester, les requêtes de type `POST`, `DELETE`, `PUT`, etc...

Il faudra alors utiliser des outils tel que [postman](https://www.postman.com/) ou encore [insomnia](https://insomnia.rest/)

## Middlewares

### C'est quoi un middleware?

Un middleware, c'est un code qui va s'exécuter entre la réception de la requête et la réponse.

Par exemple, la détection de spams, de menaces, savoir si un utilisateur est connecté et si c'est bien lui, etc... On utiliser alors un **middleware**

On peut comparer ca à un bureau de poste: Il va vérifier la conformité, la taille, etc...

Pour définir un middleware, on utilisera:
```js
app.use((req, res, next) => {
	console.log(`request mode to: ${req.url}`)
	next();
})
```

On écoutera alors sur tout les chemins. Par exemple, ce middleware fonctionnera aussi bien pour le chemin `/` que `/hello` ou encore `/prestataire/burkerking/services`

> [!example] Exemple d'utilisation
> Un système de journaux (logs) permettant d'afficher les infos sur chaque requête, utile pour débuguer en cas de crash, attaques, ...

> [!warning] Ne pas oublier `next()`
> Si on n'appelle pas `next()`, la requête ne recevra pas de réponse et va tourner à l'infini. Il est préférable de renvoyer une réponse, tel que [401 Unauthorized](https://developer.mozilla.org/fr/docs/Web/HTTP/Status/401)

### Catégories de middleware

#### 1. Middleware d'application

Un Middleware d'application permet de:
- ajouter des logs
- gérer l'authentification
- traitement commun à chaque requête
- etc...

#### 2. Middleware du router

Un middleware du router va être utilisé pour **séparer** le code.

Ainsi, sur un site d'evenementiel, on aura alors des routeurs pour chaque service, les prestataires, admins etc...

```js
const router = express.Router();

router.use((req, res, next) => {
	console.log("Middleware du router");
	next();
})

router.get("/hello", (req, res) => {
	res.send("Hello world!");
})

app.use("/api", router);
```

Dans cet exemple, le Routeur définis dans la constante `router` ne sera alors appelée que si je suis dans le chemin `/api`

Ainsi, si je fait `/api/hello`, je recevrais "Hello world!"

#### 3. Middleware de traitement d'erreur

Si une requête n'aboutis à aucun chemin connu (par exemple `/prestataire` alors qu'il n'existe pas), on aura alors une erreur 404.

Pour gérer cela, on peut alors faire:
```js
app.use("*", (req, res, next) => {
	const error = new Error("Route non trouvée");
	error.status = 404
	next(error)
})

app.get("*", (error, req, res, next) => {
	res.status(error.status || 500).json({ error: { message: error.message } })
})
```

Le **joker** `*` permet de désigner toutes les requêtes qui n'ont aboutis à aucun chemin enregistré.

#### 4. Middleware intégré

Par exemple:
```js
app.use(express.static(path.join(__dirname, "public")));

// permet d'analyser les données JSON envoyées
app.use(express.json())
```

Ces middleware sont intégrés à express, ou "natif" si on veut utiliser un terme plus précis.

#### 5. Middleware externe

Ces middleware proviennent de librairies. On retrouvera par exemple des middleware qui permettent de "parse" les cookies.


## Services

Un **service** est une fonction spécifique qui s'exécute en réponse à une requête HTTP particulière. Cette requête peut être une demande **GET** (pour récupérer des données), **POST** (pour envoyer des données), **PUT** (pour mettre à jour des données) ou **DELETE** (pour supprimer des données), entre autres.

**Un service, c'est un peu comme une recette de cuisine :**
- **L'URL** est la liste des ingrédients.
- **La méthode HTTP** (GET, POST, PUT, DELETE, etc.) est le mode de cuisson.
- **Le service** est la recette elle-même, qui indique les étapes à suivre pour préparer le plat (ici, la réponse HTTP).

### Exemple

Par exemple, imaginons le service `user.service.js`, on va alors définir plusieurs fonctions utiles, tel que:
```js
const { v4 as uuidv4 } = require("uuid");
// Imaginons qu'on a accès à une base de donnée SQL

function get_user(user_id){
	let query = database.query("SELECT * FROM users WHERE user_id = ?", user_id); // syntaxe minimalise, elle n'est pas exacte

	return query;
}

function add_user(name, age){
	let id = uuidv4();
	return database.query(
		"INSERT INTO users (id, name, age) VALUE (?, ?, ?)",
		id,
		name,
		age
	);
}

module.exports = { get_user, add_user }
```

Notez que nous ne gérons pas les requêtes. La raison que ce n'est pas la mission d'un service.

On pourra alors utiliser ce service dans les routes et middlewares de notre application `express`.