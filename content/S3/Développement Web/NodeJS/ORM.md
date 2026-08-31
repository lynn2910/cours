---
title: ORM
draft: false
tags:
---
# Définition

Un ORM (Object-Relational Mapping) est une couche entre la base de donnée et les services.

Les ORM permettent de ne pas écrire du SQL. Ces modules fournissent des méthodes pour définir les structures des tables, filtrer, trier, etc.

Cela permet de moins écrire du SQL.

---
# Sequelize

## Approches
### database-first (base de donnée d'abord)

Dans cette approche, on commence par concevoir la structure de la base de données (tables, colonnes, relations) de manière traditionnelle. Ensuite, Sequelize génère automatiquement des modèles JavaScript qui correspondent à cette structure. C'est idéal lorsque l'on travaille avec une base de données existante ou lorsqu'on préfère un contrôle total sur le schéma de la base.
### code-first (Code d'abord)

À l'inverse, le code-first consiste à définir les modèles de données directement dans le code JavaScript, en utilisant Sequelize. Ces modèles sont ensuite utilisés pour créer la structure de la base de données. Cette approche est plus flexible et permet de développer l'application et la base de données en parallèle. Elle est particulièrement adaptée aux nouveaux projets où la structure de la base de données n'est pas encore définie de manière exhaustive.

---
## Installation

Pour l'installer :
```shell
npm i -S sequelize
```

## Configuration

**Fichier de déclaration de la base de donnée :**
```js
const sequelize = new Sequelize('database', 'username', 'password', {
	host: 'localhost',
	dialect: 'mysql' | 'mariadb' | 'sqlite' | 'postgres' | 'mssql',
	pool: {
		max: 5,
		min: 0,
		idle: 10000 // 10s
	}
})
```

## Définir un modèle

```js
const User = sequelize.define('User', {
	username: {
		type: Sequelize.STRING,
		allowNull: false,
		unique: true
	},
	// ...autres champs
```

On peut alors faire,  par exemple:
```js
// Création d'un nouvel utilisateur
const newUser = await User.create({
    username: 'johnDoe',
    email: 'johndoe@example.com',
    // ... autres propriétés
});
console.log('Nouvel utilisateur créé :', newUser.toJSON());

// Recherche d'un utilisateur par son ID
const userById = await User.findByPk(newUser.id);
console.log('Utilisateur trouvé par ID :', userById.toJSON());

// Mise à jour d'un utilisateur
userById.email = 'newEmail@example.com';
await userById.save();

// Suppression d'un utilisateur
await userById.destroy();
```

## Définir des relations

### Types de relations

- **HasOne:** Un modèle a un seul élément d'un autre modèle. Par exemple, un utilisateur a une seule adresse.
- **BelongsTo:** L'inverse de HasOne. Un modèle appartient à un seul élément d'un autre modèle. Par exemple, une adresse appartient à un seul utilisateur.
- **HasMany:** Un modèle a plusieurs éléments d'un autre modèle. Par exemple, un utilisateur a plusieurs commandes.
- **BelongsToMany:** Une relation plusieurs-à-plusieurs, nécessitant une table intermédiaire. Par exemple, un utilisateur peut avoir plusieurs rôles, et un rôle peut être attribué à plusieurs utilisateurs.

#### HasOne

Un modèle a une seule instance d'un autre modèle. C'est une relation un-à-un.
```js
// Un utilisateur a une seule adresse
User.hasOne(Address);
```
#### BelongsTo

L'inverse de HasOne. Un modèle appartient à une seule instance d'un autre modèle.

```js
// Une adresse appartient à un seul utilisateur
Address.belongsTo(User);
```
#### HasMany

Un modèle peut avoir plusieurs instances d'un autre modèle. C'est une relation un-à-plusieurs.

```js
// Un utilisateur peut avoir plusieurs commandes
User.hasMany(Order);
```
#### BelongsToMany

Une relation plusieurs-à-plusieurs, nécessitant une table intermédiaire pour gérer les associations.
```js
// Un utilisateur peut avoir plusieurs rôles, et un rôle peut être attribué à plusieurs utilisateurs
User.belongsToMany(Role, { through: 'UserRoles' });
Role.belongsToMany(User, { through: 'UserRoles' });
```

## Requêtes

On peut trouver tous ces opérateurs :
- **findAll()**: Récupère tous les enregistrements d'un modèle.
- **findOne()**: Récupère le premier enregistrement correspondant à un critère.
- **findByPk()**: Récupère un enregistrement par sa clé primaire.
- **create()**: Crée un nouvel enregistrement.
- **update()**: Met à jour un enregistrement existant.
- **destroy()**: Supprime un enregistrement.

### Exemples

**Récupérer les utilisateurs qui ont plus de 30 ans**
```js
// Trouver tous les utilisateurs dont l'âge est supérieur à 30
const users = await User.findAll({
    where: {
        age: { [Op.gt]: 30 }
    }
});
```

**Récupérer tout les utilisateurs avec leurs commandes**
```js
// Récupérer tous les utilisateurs avec leurs commandes
const usersWithOrders = await User.findAll({
    include: [{
        model: Order
    }]
});
```

**Compter le nombre de commandes de chaque utilisateur**
```js
// Compter le nombre de commandes par utilisateur
const ordersByUser = await Order.findAll({
    attributes: ['userId', [Sequelize.fn('COUNT', 'id'), 'totalOrders']],
    group: ['userId']
});
```

**Trier les utilisateurs par age descendant**
```js
// Trier les utilisateurs par âge décroissant
const usersSorted = await User.findAll({
    order: [['age', 'DESC']]
});
```

**Pour limiter le nombre de résultats et gérer la pagination**
```js
// Récupérer les 10 premiers utilisateurs, en commençant à partir du 20ème
const usersPaginated = await User.findAll({
    limit: 10,
    offset: 20
});
```

### Utiliser des requêtes SQL

Il suffit de faire :
```js
const result = await sequelize.query('SELECT * FROM users WHERE age > 30');
```

