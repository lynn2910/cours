---
title: Introduction
draft: false
tags:
---
Dans la réalisation de logiciel, les grandes phases de la réalisation d'un système logiciel sont très proches de ce qui se fait pour le bâtiment.

Mais la métaphore avec le BTP (voir *[[Architectures logicielles et objets]]*) est "mou" pour permettre la réusabilité, les évolutions etc. Et c'est là que les problèmes commencent.

# La construction logicielle

La conception abstrait les détails :
- avec l'emploi d'une notation graphique pour la structure et le comportement ;
- les détails du code n'apparaissent pas ;

Les développeurs doivent être capables de lire les "plans" et de compléter les détails (aka. le code).


# Buts et règles de la conception par objets

## Définition des critères de qualité

*Définitions extraites du standard **ISO 9126***.

**La fonctionnalité :** Dans quelle mesure le système rend-il le service attendu ?

**Robustesse :** Comment le système réagit-il hors du périmètre prévu ? (inclut des morceaux de cybersécurité).

**Maintenabilité :** Le système est-il facile à tester, à corriger ?

**Réutilisabilité :** Le système est-il facile à modifier lorsque les besoins changent ou son environnement change ?

> [!quote] The primary purpose of architecture is to support the life cycle of the system. Good architecture makes the system easy to understand, easy to develop, easy to maintain, and easy to deploy. The ultimate goal is to minimise the lifetime cost of the system and to maximise programmer productivity.

### Principes de bases

Il existe de nombreux principes :
- SOLID
- DRY
- YAGNI
- KISS
- CBD

#### SOLID



## Principes généraux qui guident les décisions de conception