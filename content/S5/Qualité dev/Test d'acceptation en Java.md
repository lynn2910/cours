---
title: Test d'acceptation en Java
draft: false
tags:
---
# Définition

Un test d'acceptation est un test métier permettant de valider tout ou partie d'une fonctionnalité.

Les tests d'acceptation permettent au client de vérifier qu'une fonctionnalité a été implémentée. Si l'ensemble des tests d'acceptation d'une fonctionnalité sont verts, le client peut accepter la fonctionnalité.

Par nature, ce sont des **tests fonctionnels**.

# Acteurs du test d'acceptation

Le **client** définit la fonctionnalité à implémenter et les tests d'acceptation associés.

Le **développeur** code l'application et les fixtures permettant de réaliser le lien entre les tests d'acceptation et le code.

# Agilité et tests d'acceptation

Les méthodes agiles utilisent les cycles de développement courts pendant lesquels sont pris en charge la réalisation de "stories". La définition et la "mise en page" des tests d'acceptation prennent naturellement place avant de commencer l'implémentation relative à une story.

> **ATDD :** Acceptance Test Driven Development

# Outils du Test d'acceptation


| Outil                | FitNesse      | Concordion          | Cucumber            |
| -------------------- | ------------- | ------------------- | ------------------- |
| **Interface saisie** | wiki - tables | html - div Example  | teste - Gherkin     |
| **Interprétation**   | serveur dédié | Junit               | Junit               |
| **Résultats**        | dans le wiki  | pages html résultat | pages html résultat |

