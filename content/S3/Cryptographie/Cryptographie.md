---
title: Cryptographie
draft: false
tags:
---
La cryptographie possède deux composantes principales:
- Méthodes cryptographiques
- Méthode non-cryptographiques

## Méthodes cryptographiques

Il existe trois méthodes de chiffrement:
- [[Chiffrement symétrique]] (Même clé)
- [[Chiffrement asymétrique]] (Clé privé-clé publique)
- [[Sans-clé]] (méthodes de hachages...)

## Méthodes non-cryptographiques

On y retrouvera notamment:
- Sécurité physique
- Logique

## Confidentialité

Intégrité des données: infos non changées, s'assurer que les données n'ont pas été manipulées

Disponibilité du service: Se prémunir contre les attaques suivantes:
- [[DoS & DDoS]]
- ...
## Sécurité

Tout les algorithmes peuvent être analysés (voir [[Analyse de Sécurité]])

Afin de protéger au mieux les informations, il faut:
- Une confusion suffisante afin d'empêcher une attaque linéaire
- L'uniformité
- Une taille de clés assez importante

Un algorithme de chiffrement par bloc permet de construire un algorithme de chiffrement par flux.

