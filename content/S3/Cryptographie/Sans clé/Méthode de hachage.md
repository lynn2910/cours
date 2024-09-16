---
title: Méthode de hachage
draft: false
tags:
---
## Définition

Le hachage est une opération qui consiste à transformer une donnée d'une taille quelconque (un fichier, un message, un mot de passe) en une chaîne de caractères de longueur fixe, appelée hache ou empreinte digitale. Cette chaîne est unique pour une donnée donnée et est généralement beaucoup plus courte que la donnée d'origine.

## Utilisations

Il y a plusieurs utilisations:
- **Vérification d'intégrité:** Assurer qu'un fichier n'a pas été modifié en comparant son hache avant et après un transfert.
- **Stockage sécurisé de mots de passe:** Stocker les hachés des mots de passe plutôt que les mots de passe en clair, rendant une attaque par force brute beaucoup plus difficile.
- **Indexation rapide:** Créer des tables de hachage pour une recherche efficace dans de grandes bases de données.
- **Signatures numériques:** Vérifier l'authenticité d'un message.

## Propriétés d'une bonne fonction de hachage

Une bonne fonction de hachage doit satisfaire plusieurs propriétés :
- **Déterminisme:** La même entrée doit toujours produire la même sortie.
- **Rapidité:** Le calcul du hachage doit être rapide, même pour de grandes entrées.
- **Résistance aux collisions:** Il doit être extrêmement difficile de trouver deux entrées différentes qui produisent la même sortie (collision).
- **Avalanche:** Un petit changement dans l'entrée doit entraîner un changement important dans la sortie.

## Types de fonctions de hachages

Les plus connues sont:
- **MD5 (Message Digest 5):** Bien que rapide, il est considéré comme obsolète en raison de faiblesses en termes de résistance aux collisions.
- **SHA-1 (Secure Hash Algorithm 1):** Semblable à MD5, mais plus sécurisé. Cependant, des collisions ont également été trouvées, le rendant moins sûr pour de nouvelles applications.
- **SHA-256, SHA-512:** Des versions plus récentes et plus sécurisées de SHA-1, largement utilisées.
- **SHA-3:** Un nouveau standard conçu pour remplacer SHA-2, offrant une sécurité encore plus élevée.
- **Blake2:** Une famille d'algorithmes de hachage performants et sécurisés, souvent utilisés dans les blockchains.

## Applications Pratiques

Exemples:
- **Systèmes de fichiers:** Pour vérifier l'intégrité des fichiers.
- **Systèmes de contrôle de version:** Pour détecter les modifications entre les versions d'un fichier.
- **Protocoles de communication:** Pour assurer l'intégrité des données transmises.
- **Cryptomonnaies:** Pour sécuriser les transactions et créer des blocs dans les blockchains.

## Attaques et Contres-mesures

Il y a plusieurs attaques:
- **Attaque par force brute:** Essayer toutes les entrées possibles jusqu'à trouver une collision. Pratiquement impossible pour des fonctions de hachage bien conçues.
- **Attaque par anniversaire:** Exploite les probabilités pour trouver des collisions plus rapidement que par force brute. Les fonctions de hachage modernes sont conçues pour résister à cette attaque.
- **Attaque par extension de longueur:** Exploite des faiblesses dans la conception de certaines fonctions de hachage.

Approfondir: [wikipedia.org](https://fr.wikipedia.org/wiki/Fonction_de_hachage)

