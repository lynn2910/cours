---
title: Cookies & Session
draft: false
tags:
---
# Cookies

Les cookies sont un espace de stockage local.

Il existe 3 type de stockages :
- [[#Stockage de session]]
- [[#Stockage local]]
- [[#Stockage de cookies]]

## Ce que dit la loi

Les cookies doivent être optionnels quand ces derniers ne sont pas **requis**.
De ce fait, des cookies pour se connecter ne peuvent pas être refusés, mais par exemple les cookies publicitaires, **doivent** être optionnels.

## Utilisation

Les cookies permettent :
- de tracer un utilisateur
- de personnaliser les publicités d'un utilisateur
- 

---

# Stockages

Le navigateur offre plusieurs mécanismes pour stocker des données localement, permettant ainsi de personnaliser l'expérience utilisateur et de conserver certaines informations entre différentes visites. Parmi ces mécanismes, les cookies sont les plus connus, mais il existe d'autres options plus modernes et flexibles.
# Stockage de session

Le stockage de session est une zone de mémoire temporaire allouée à chaque onglet ou fenêtre du navigateur. Les données stockées dans le **sessionStorage** sont accessibles uniquement pendant la durée de la session de navigation en cours, c'est-à-dire tant que l'onglet ou la fenêtre reste ouvert. À la fermeture de celle-ci, les données sont automatiquement effacées.

**Utilisations typiques :**
- **États temporaires d'une application web :** Par exemple, pour mémoriser les choix d'un utilisateur au cours d'une session sans avoir à les renvoyer à chaque requête au serveur.
- **Panier d'achat :** Le contenu du panier est généralement stocké en sessionStorage pour être accessible tout au long de la navigation de l'utilisateur sur le site d'e-commerce.
---
# Stockage local

Le stockage local offre une persistance plus longue que le stockage de session. Les données stockées dans le **localStorage** sont conservées **sur l'appareil** de l'utilisateur, même après la fermeture du navigateur, et ce, jusqu'à ce qu'elles soient supprimées explicitement ou par l'utilisateur lui-même.

**Utilisations typiques :**
- **Préférence utilisateur :** Les paramètres de personnalisation d'un site (thème, langue, etc.) peuvent être enregistrés en **localStorage** pour être restaurés lors de la prochaine visite.
- **Données d'authentification :** Certaines applications peuvent stocker des jetons d'authentification en **localStorage** pour éviter de demander à l'utilisateur de se reconnecter à chaque fois.

---
# Stockage de cookies

Les cookies sont de petits fichiers texte stockés sur l'appareil de l'utilisateur par le navigateur web. Ils sont envoyés par un serveur web et renvoyés à chaque requête subséquente, permettant ainsi aux serveurs de "se souvenir" d'informations spécifiques à l'utilisateur, comme ses préférences ou le contenu de son panier d'achat.

## Structure d'un cookie

Un cookie est généralement composé de plusieurs éléments :

- **Nom :** Un identifiant unique qui permet d'identifier le cookie.
- **Valeur :** Les données associées au cookie, sous forme de chaîne de caractères.
- **Domaine :** Le domaine pour lequel le cookie est valide.
- **Chemin :** Le répertoire du site pour lequel le cookie est valide.
- **Durée de vie :** La durée pendant laquelle le cookie sera conservé sur l'appareil de l'utilisateur (en secondes, minutes, heures, jours, etc.).
- **Sécurité :** Indique si le cookie doit être transmis uniquement sur une connexion sécurisée (HTTPS).

## Définir des cookies

Pour définir un cookie, on utilise généralement des méthodes fournies par l'objet `document` en JavaScript. Par exemple, pour créer un cookie nommé `monCookie` avec la valeur `maValeur` et une durée de vie de 1 jour :
```js
document.cookie = "monCookie=maValeur; expires=" + new Date(Date.now() + 86400000).toUTCString();
```

---

# Utiliser express-session et cookie-session

Dans le contexte de Node.js et du framework Express, les modules `express-session` et `cookie-session` sont couramment utilisés pour gérer les sessions utilisateur.

## express-session

- **Principe :** Stocke les données de session sur le serveur et envoie uniquement un identifiant de session (session ID) dans un cookie.
- **Avantages :** Plus sécurisé, car les données sensibles ne sont pas exposées dans le cookie.
- **Inconvénients :** Nécessite une gestion supplémentaire des sessions sur le serveur.

**Exemple :**
```js
const express = require('express');
const session = require('express-session');

const app = express();

app.use(session({
  secret: 'votre_clé_secrète',
  resave: false,
  saveUninitialized: false
}));

app.get('/', (req, res) => {
  if (req.session.user) {
    res.send(`Bienvenue, ${req.session.user}!`);
  } else {
    req.session.user = 'John Doe';
    res.send('Vous êtes connecté en tant que John Doe.');
  }
});
```

## cookie-session

- **Principe :** Stocke directement les données de session dans le cookie.
- **Avantages :** Simple à mettre en œuvre.
- **Inconvénients :** Moins sécurisé, car les données sont directement exposées dans le cookie.

**Exemple:**
```js
const express = require('express');
const cookieSession = require('cookie-session');

const app = express();

app.use(cookieSession({
  name: 'session',
  keys: ['clé1', 'clé2']
}));

app.get('/', (req, res) => {
  if (req.session.user) {
    res.send(`Bienvenue, ${req.session.user}!`);
  } else {
    req.session.user = 'John Doe';
    res.send('Vous êtes connecté en tant que John Doe.');
  }
});
```

## Choisir entre express-session et cookie-session

Le choix entre `express-session` et `cookie-session` dépend de plusieurs facteurs :

- **Sécurité :** Si la sécurité est une priorité, `express-session` est généralement préférable.
- **Complexité :** Si vous souhaitez une solution simple et rapide à mettre en œuvre, `cookie-session` peut être un bon choix.
- **Volume de données :** Pour de grandes quantités de données, `express-session` est plus adapté.

---
# Cross-origin Resource Sharing (CORS)

Les CORS sont un mécanisme de **sécurité** qui permet à un navigateur d'accéder à des ressources (comme des images, des feuilles de style, des scripts, etc.) d'un domaine différent de celui à partir duquel la page a été chargée. Cela est particulièrement utile pour les applications web modernes qui font appel à des API externes ou à des composants d'autres domaines.

Sans CORS, la politique de même origine (Same-Origin Policy) empêcherait les navigateurs d'effectuer des requêtes vers des domaines différents, ce qui limiterait considérablement les fonctionnalités des applications web.

## Cas d'utilisation des CORS

- **API REST:** Consommer des API REST externes pour récupérer des données.
- **Intégration de services tiers :** Intégrer des services comme Google Maps, Facebook, etc.
- **Développement d'applications monopage (SPA):** Les SPA font souvent appel à des API pour récupérer des données dynamiquement.

## Problèmes courants liés aux CORS

- **Blocage des requêtes :** Si le serveur ne configure pas correctement les en-têtes CORS, le navigateur bloquera la requête.
- **Erreurs de préflight:** Des erreurs peuvent survenir lors de la requête OPTIONS si les paramètres ne sont pas correctement définis.
- **Sécurité :** Il est important de configurer correctement les CORS pour éviter les problèmes de sécurité.
## Configurer les CORS sur Express

La configuration des CORS se fait généralement côté serveur. Voici un exemple de configuration basique avec Express.js :

```js
const express = require('express');
const cors = require('cors');

const app = express();

app.use(cors({
  // Autoriser uniquement les requêtes provenant de https://exemple.com
  origin: 'https://exemple.com', 
  // Autoriser les méthodes GET, POST, PUT et DELETE
  methods: 'GET, POST, PUT, DELETE', 
  // Autoriser les en-têtes Content-Type et Authorization
  allowedHeaders: ['Content-Type', 'Authorization'], 
}));

// ...
```