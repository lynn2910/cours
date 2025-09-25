---
title: Redis
draft: false
tags:
---
# La base de donnée Redis

## Introduction

Redis (_REmote DIctionary Server_) est une base de données **in-memory** orientée **clé-valeur**: toutes les données sont stockées en mémoire vive plutôt que sur disque, ce qui rend Redis **extrêmement rapide** pour lire et écrire, mais rend les données temporaires.

Redis est :
- **Open source** et très largement utilisé.
- **Multi-types de données** : chaînes, listes, ensembles, tables de hachage, ensembles triés.
- **Ultra-performant** : on parle de centaines de milliers d’opérations par seconde.
- **Polyvalent** : cache, gestion de sessions, file d’attente, système de messagerie (_pub/sub_), classements, etc.

Redis n’est pas un substitut parfait à une base relationnelle (genre MySQL) ou documentaire (genre MongoDB). C’est plutôt un **complément** spécialisé dans la rapidité et la simplicité d’accès.

## Installation et démarrage

### 1. Lancer un conteneur Docker

Afin d'installer Redis, la recommandation est d'utiliser Docker :
```sh
docker run -d  --name redis  -p 6379:6379 redis:latest
```

### 2. Vérifier que redis tourne

On peut ensuite vérifier qu'il tourne :
```sh
docker ps
```

Ensuite, on peut faire :
```sh
docker exec -it redis redis-cli
```

et entrer dans le cli de Redis: 
```redis
PING
```

Le serveur devrait nous répondre `PONG`.

## Type de données

Redis n’est pas limité à stocker une simple chaîne de caractères. Il propose plusieurs structures intégrées, chacune avec ses propres commandes.

### 1. Strings

C’est le type le plus simple : une clé, une valeur (texte ou binaire).
```R
SET user:1 "Alice"
GET user:1
```

Un cas typique : garder en cache le résultat d’une requête SQL lourde. Tu balances ça dans Redis comme une simple chaîne, et tu l’éjectes quand ce n’est plus utile.

On peux aussi faire des opérations numériques :
```R
SET counter 10
INCR counter   # => 11
DECR counter   # => 10
```

Ici, on imagine un compteur de visites sur une page web. Plutôt que d’infliger des écritures permanentes à une base SQL, Redis encaisse les incréments à la volée.
### 2. Lists

Ce sont des listes ordonnées, parfaites pour gérer des files (*FIFO* ou *LIFO*).

```R
LPUSH tasks "task1"
LPUSH tasks "task2"
LRANGE tasks 0 -1   # ["task2", "task1"]

RPOP tasks   # retire et retourne "task1"
```

On retrouve ça quand on veut stocker des *notifications* dans l’ordre avec lequel elles arrivent, ou pousser des jobs dans une file de traitement. Certains s’en servent aussi comme historique de logs : on empile les événements et dépile à mesure qu’ils sont consommés.

### 3. Sets

Ce sont des ensembles non ordonnés de valeurs uniques.

```R
SADD tags "redis" "database" "cache"
SMEMBERS tags   # {"redis", "database", "cache"}
SISMEMBER tags "redis"   # 1 (oui)
```

Idéal pour gérer des abonnements ou des étiquettes.

Un exemple concret : stocker les rôles d’un utilisateur (`admin`, `editor`, `viewer`). On ne veux pas de doublons, mais on veux tester rapidement si un rôle existe. Redis fait ça les doigts dans le nez.

### 4. Hashes

Les Hashes sont des tables clé/valeur à l’intérieur d’une clé (parfait pour des objets).

```R
HSET user:1 name "Alice" age "30"
HGET user:1 name       # "Alice"
HGETALL user:1         # {name: "Alice", age: "30"}
```

C’est exactement ce qu'on doit utiliser quand on veut stocker un petit profil utilisateur sans taper dans une base lourde. Nom, âge, email, préférences… tout est regroupé sous une même clé. Ça remplace très bien une table SQL quand on veux juste de la lecture/écriture ultra-rapide.

### 5. Sorted Sets (ZSets)

Comme un [[#3. Sets|Sets]], mais chaque valeur a un score numérique qui définit l’ordre. Idéal pour un classement par exemple.

```R
ZADD leaderboard 100 "Alice"
ZADD leaderboard 200 "Bob"
ZRANGE leaderboard 0 -1 WITHSCORES # => ["Alice", 100, "Bob", 200]
```

C’est la structure par excellence pour gérer un classement, comme le tableau des scores d’un jeu vidéo ou un ranking de popularité. Le score détermine la position, et Redis se charge de tout garder trié en permanence, même quand on insère de nouveaux joueurs.

## Fonctionnalités avancées

### Expiration des clés

Chaque clé peut avoir une durée de vie limitée. Redis supprime automatiquement la donnée une fois le temps écoulé.

```R
SET session:123 "data"
EXPIRE session:123 60   # expire après 60 secondes
TTL session:123         # retourne le temps restant
```

C’est pratique pour gérer des sessions utilisateurs ou des caches temporaires. Inutile d’écrire du code qui nettoie, Redis s’en charge.

### Transactions

Redis permet d’exécuter plusieurs commandes de façon atomique grâce à `MULTI` et `EXEC`.

```R
MULTI
INCR balance:alice
DECR balance:bob
EXEC
```

Soit tout passe, soit rien ne passe. Les transactions sont utiles lorsqu’on manipule plusieurs clés liées, comme des comptes ou des inventaires, et qu’il faut garantir la cohérence.

### Pipelines

Lorsqu’on enchaîne beaucoup de commandes, les envoyer une par une crée des allers-retours réseau coûteux. Avec un pipeline, on envoie tout d’un bloc et on récupère toutes les réponses d’un coup.

C’est particulièrement utile pour charger ou lire des gros volumes de données en un temps minimal.

### Pub/Sub : un abonnement sous stéroïdes

Redis peut servir de système de messagerie. On publie des messages dans un canal, et tous les abonnés les reçoivent en temps réel.

```R
SUBSCRIBE news
PUBLISH news "Redis 7 released!"
```

C’est une solution élégante pour construire un _chat de messagerie_, de la *diffusion d’événements*, ou encore des *microservices* qui communiquent sans passer par une API lourde.

### Scripts Lua

Pour les besoins plus complexes, on peut exécuter des scripts Lua directement dans Redis. Cela permet d’exécuter de la logique côté serveur sans multiplier les allers-retours.

Exemple simplifié :
```R
EVAL "return redis.call('GET', KEYS[1])" 1 mykey
```

On gagne en performance et en cohérence, surtout quand il s’agit de traitements qui impliquent plusieurs opérations en une seule étape.

### Sauvegardes et persistences

Même si Redis vit en mémoire, il propose deux mécanismes pour conserver les données :
- **RDB** : des snapshots réguliers, rapides à charger.
- **AOF** (_Append Only File_) : un journal d’écriture plus précis, au prix d’un peu de performance.

Dans un environnement de production, on choisit souvent un mélange des deux pour ne pas tout perdre au redémarrage.


## Best practices

### Nommage des clés

Il est crucial d’avoir une convention claire pour les clés. On utilise souvent le format `objet:id:propriété`.

```R
user:123:profile
session:abc123
cache:products:42
```

Cela permet de retrouver facilement les clés et d’éviter les collisions, surtout quand plusieurs équipes ou *microservices* écrivent dans la même instance Redis.

### 2. Gérer l’expiration

Pour les caches et sessions, définir un TTL (_time to live_) évite d’accumuler des données obsolètes et de saturer la mémoire.

```R
SET cache:product:42 "..." EX 3600   # expire au bout d’une heure
```

Pour des données critiques, on peut combiner TTL et mécanismes de sauvegarde pour ne rien perdre.

### 3. Attention à la mémoire

Redis vit en mémoire, donc une clé énorme ou un JSON géant peut faire exploser l’instance. Il vaut mieux :
- Stocker les objets en petits morceaux (hashes, sets) plutôt qu’en gros blobs.
- Surveiller la mémoire avec `INFO MEMORY`.
- Configurer un `maxmemory` et une stratégie d’éviction (`volatile-lru`, `allkeys-lru`, etc.) pour éviter les crashes.

### 4. Utiliser des structures adaptées

Chaque type de données a son usage. Stocker un objet complexe dans un *string* alors qu’un *Hash* serait plus approprié est une erreur fréquente. Utiliser le bon type permet d’écrire moins de code et d’améliorer les performances.

> [!example] Les JSON
> Une solution naïve serait de transformer un objet JSON en String et le stocker dans Redis tel-quel. C'est la pire idée qui est imaginable.
> Il faut à la place utiliser un [[#4. Hashes|HSET]] afin de rendre le système plus évolutif, optimisé et performant.

### 5. Eviter les opérations bloquantes

Certaines commandes comme `KEYS *` parcourent toutes les clés et peuvent bloquer Redis. Il vaut mieux utiliser :
- `SCAN` pour parcourir les clés de façon incrémentale.
- `HSCAN`, `SSCAN`, `ZSCAN` pour les structures spécialisées.

### 6. Surveillance et monitoring

Des outils comme **RedisInsight** ou **Grafana + Redis exporter** aident à anticiper les problèmes avant qu’ils ne deviennent critiques.
