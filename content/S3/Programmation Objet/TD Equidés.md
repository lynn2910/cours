---
title: TD Equidés
draft: false
tags:
---
## Sujet

Les Equidés sont général:
- Bardots
- Chevaux
- Anes
- Mules

Ces 4 class sont des filles/sous-classes de `Equides`

Au 3e niveau:
- **Bardots:** Bardot, Bardine
- **Chevaux**: Etalon, Jument
- **Anes:** Ane, Anesse
- **Mules:** Mulet, Mule

On retrouve donc ce schéma:
![[Pasted image 20240924090217.png]]

## Règles

### Attributs

Les Equidés doivent avoir les propriétés suivantes: `force`, `endurance`, `age`, `esperanceVie`

En moyenne, un male a une force de 6, une endurance de 4.
Pour une femelle, en moyenne, la force est de 4 et l'endurance de 6.

Règles:
- **Chevaux:** force moyenne augmentée de 1 
- **Bardots:** force moyenne diminuée de 1
- **Anes et Mules**: endurance moyenne augmentée de 1
- **Chevaux:** endurance moyenne diminuée de 1
- Pour un individu donné (une instance de class), **force** et **endurance** varient de plus ou moins 2. (tiré au sort)
- **esperanceVie**: La moyenne est de 20 ans. Sauf les anes et mules qui est de 25 ans

### Méthodes

Les méthodes suivantes sont requises:
- `Equide rencontre(Equide e)`
- `int courir(); renvoie le nb de km qu'il peut parcourir par jour`
- `int tracter(); renvoie le poids qu'il peut tracter`


Règles:
- **Méthode** `rencontre`:
	- Bardots et Mules sont stériles. 
	- Les autres sont fertiles à partir de 3 ans, les femelles arrêtent à 18 ans
	- Deux équidés fertiles de sexe différent de même type produisent un individu
	- Deux équidés de type différent ne se reproduisent pas (sauf Anes - Jument = Mulet/Mules, et Etalon - Anesse = Bardots/Bardines)
- **Méthode** `tracter`:
	- Renvoie 50x la force
	- Sauf les mules qui renvoient 30x la force
- **Méthode** `courir`:
	- Renvoie 10x l'endurance
	- Sauf les bardots, mules et anes qui renvoient 15x l'endurance
	- Sauf les bardines qui renvoient 12x l'endurance

## Objectif

L'objectif est d'écrire la structure, les class et les méthodes tout en respectant le principe de polymorphisme

| Classe  | Rencontre()   | Tracter() | courir() |
| ------- | ------------- | --------- | -------- |
| Equide  | Abstraite     |           |          |
| Chevaux | Héritée (abs) |           |          |
| Bardots | Définie       |           |          |
| Anes    | Héritée (abs) |           |          |
| Mules   | Définie       |           |          |
| Etalon  | Définie       |           |          |
| Jument  | Définie       |           |          |
| Ane     | Définie       |           |          |
| Anesse  | Définie       |           |          |
| Mulet   | Héritée       |           |          |
| Mule    | Héritée       |           |          |
| Bardot  | Héritée       |           |          |
| Bardine | Héritée       |           |          |
