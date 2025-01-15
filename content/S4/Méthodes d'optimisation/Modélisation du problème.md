---
title: Modélisation du problème
draft: false
tags:
---
# Exemple : production de ressources


## Définitions des besoins

Cette étape se fait en 3 étapes :
1. Variables de décisions
2. Contraintes
3. Fonction-objectif

###  Variables de décisions

|                   | Quantité de fer (Kg) | Conso. d'énergie | Temps de prod. |
| ----------------- | -------------------- | ---------------- | -------------- |
| Lingot d'acier LQ | 2 Kg                 | 4 Kw·h           | 3h             |
| Lingot d'acier HQ | 1 Kg                 | 5 Kw·h           | 10h            |
La production se fait par lot de 1000 lingots.

### Contraintes

**Les contraintes** de l'entreprise sont basées sur **les ressources**

L'entreprise peut posséder :
- 8 Tonnes de fer
- 20 000 Kw·h
- 30 000 Heures

### Fonction-objectif

> [!todo] Problème: COMBIEN de lingots de chaque type faut-il produire pour maximiser le chiffre d'affaire ?

## Formalisation du programme

### Variables

On cherche :
- $x_1$ = nb de lots de 1000 lingots de type LQ
- $x_2$ = nb de lots de 1000 lingots de type HQ

Les variables $x_1$ et $x_2$ 

### Contraintes 

Quantité de fer : 
	$2000x_1 + 1000x_2 <= 8000$
	Donc:
	$2x_1 + x_2 <= 8$

Conso. d'énergie :
	$4000x_1 + 5000x_2 <= 20 000$
	$4x_1 + 5x_2 <= 20$

Temps :
	$3x_1 + 10_x2 <= 30$

## Solution

Nous avons alors cet ensemble à résoudre :
$$
\left\{
    \begin{array}{ll}
		2x_1 + x_2 <= 8 \\
		4x_1 + 5x_2 <= 20 \\
		3x_1 + 5x_2 <= 30
    \end{array}
\right.
$$
### Représentation graphique

> [!todo] faut que je pique le graph sur son diapo, j'arrive pas à le reproduire

### Critères d'optimisation - Fonction-objectif


Le programme $(x_1, x_2)$ doit **maximiser** le chiffre d'affaires : 

$\max_\limits{(x_1, x_2)} Z = 300_x1 + 800x_2$

On appelle **solution optimale** toute solution admissible $(x_1^*, x_2^*)$ **optimisant** la fonction-objectif :

$\forall (x_1, x_2) Z = 300x_1 + 800x_2 <= Z^* = 300x_1^* + 800x_2^*$

$\Delta : 300x_1 + 800x_2 = Z$

## Méthode graphique

> [!todo] pareil, me manque les graphs :(

**Point D :**
$$
\left\{
	\begin{array}{ll}
		x_2 = 0 \\
		2x_1 + x_2 = 8
	\end{array}
	=
	\begin{array}{ll}
		x_2 = 0
		2x_1 = 4
	\end{array}
	=> Z = 1200 K
\right.
$$

> [!summary] On utilise la même logique pour tout les points

