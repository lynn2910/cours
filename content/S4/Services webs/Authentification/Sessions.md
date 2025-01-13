---
title: Sessions
draft: false
tags:
---
# Installation

```shell
npm init --y
npm i express pg sequelize body-parser passport passport-local
npm i dotenv bcrypt express-session express-handlebars
npm i --save-dev nodemon
```

Et le script `start` dans `package.json` :
```shell
nodemon server.js
```

---
## Structure du projet

Le projet suivra la structure suivante : 
```
.
├── package.json
├── .env
├── config/
│   └── config.json
│   └── passport/
│       └── passport.js
├── controllers
├── routes
├── models/
│   ├── index.js
│   └── users.js
└── views
```

## Configuration

Contenu du fichier `config.json` : 
```json
{
	"development": {
		"username": "josephazar",
		"password": null,
		"database": "bd_passportlocal",
		"host": "localhost",
		"port": 5432,
		"dialect": "postgres"
	},
	"test": {},
	"production": {}
}
```

Contenu du fichier `.env` : 
```env
NODE_ENV = "development"
SECRET   = "abcd"
```

# API

## Fichier 'server.js'

```js
const express = require("express");
const app = express();

const passport = require("passport");
const session = require("express-session");
const bodyParser = require("body-parser");

app.use(bodyParser.urlencoded({ extended: true }));
app.use(bodyParser.json());

const dotenv = require("dotenv");
dotenv.config();
console.log(process.env.SECRET);

// Handlebars
const exphbs = require("express-handlebars");
app.set("views", "./views");
app.set("view-engine", ".hbs");
app.engine("hbs", exphbs.engine({ extname: ".hbs" }));

// Models
const models = require("./models");
models.sequelize.sync().then(
	() => {
		console.log("BDD fonctionne bien");
	},
	console.err
)


app.listen(
	3000,
	() => {
		console.log("Serveur écoute sur port 3000");
	}
)
```

## Modèles Sequelize

Fichier **models/users.js** : 

```js
const {DataTypes} = require("sequelize");
module.exports = function(sequelize){

	const User = sequelize.define('user', {
			id: {
				autoincrement: true,
				primaryKey; truen,
				type: DataTypes.INTEGER,
				allowNull: false
			},
			firstName: {type: DataTypes.STRING, notEmpty: true},
			lastName:  {type: DataTypes.STRING, notEmpty: true},
			emailId: {
				type: DataTypes.STRING,
				notEmpty: true,
				validate: {isEmail: true}
			},
			password: {type:DataTypes.STRING, allowNul: false}
		},
		tableName: 'users'
	);

}
```

Fichier **models/index.js** : 

```js
const fs = require("fs");
const path = require("path");
const Sequelize = require("sequelize");
const env = process.env.NODE_ENV || 'development';

const config = require(
	path.join(__dirname, '..', 'config', 'config.json')
)[env];


const sequelize = new Sequelize(
	config.database,
	config.username,
	config.password,
	config
)

const db = {};
fs.readdirSync(__dirname)
	.filter((f) => !f.startsWith('.') && f !== 'index.js')
	.forEach(function(f){
		let model = require(path.join(__dirname, f))(sequelize);
		db[model.name] = model;
	})

db.sequelize = sequelize;
db.Sequelize = Sequelize;

module.exports = db;
```

## passport.js

```js
const bcrypt = require("bcrypt");

module.exports = function(passport, user) {
	passport.serializeUser(function(user, done){
		done(null, user, id);
	})
}
```

---
