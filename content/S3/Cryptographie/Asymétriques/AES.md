---
title: AES
draft: false
tags:
---
Source: [wikipedia.org](https://fr.wikipedia.org/wiki/Advanced_Encryption_Standard)

![[langfr-330px-AES-SubBytes.svg.png]]

## Chiffrement

Pour chaque tour (ou itération) de chiffrement, une **clé de tour** différente est utilisée. Ces clés de tour sont générées à partir de la **clé principale** grâce à un processus appelé **expansion de clés**.

La **seule** opération qui utilise directement la clé principale est la **première** génération de clé de tour. Les clés de tour suivantes sont dérivées de la clé de tour précédente.
## Clés

Au départ, les clés AES étaient constituées de 256 bits

## Effet d'avalanche

L'effet d'avalanche définie que tout changement dans la clé ou dans le texte éclair(même mineur) doit changer plus de 50% de différences dans le texte entre les deux représentations binaires.

Plus d'infos: [wikipedia.org](https://fr.wikipedia.org/wiki/Effet_avalanche)

## Problèmes

### Attaques physiques

Les avancées en matière d'attaques par canaux auxiliaires ont montré que des informations physiques, telles que la consommation énergétique ou les émissions électromagnétiques, pouvaient être **exploitées** pour **déduire la clé secrète** utilisée dans un algorithme de chiffrement comme AES.

Dès les années 2000, il est devenu évident que la sécurité d'une implémentation cryptographique ne se résumait plus à la robustesse mathématique de l'algorithme lui-même. Les chercheurs ont ainsi développé des **contre-mesures** pour rendre les implémentations **plus résistantes à ces attaques**.

Par exemple, des techniques de masquage ont été introduites pour rendre la consommation d'énergie moins corrélée à la valeur des données manipulées. De plus, des opérations inutiles, mais calculées, ont été ajoutées pour brouiller les traces laissées par les calculs cryptographiques.

### Implémentations

Bien qu'un algorithme puisse être mathématiquement solide, sa vulnérabilité aux attaques réside souvent dans les détails de son implémentation. Pour garantir une sécurité optimale, il est essentiel de prendre en compte les techniques de cryptanalyse et de mettre en œuvre des contre-mesures adaptées.