---
title: "TP2: Serveur de fichiers"
draft: false
tags:
---
## Partie 1 : lire des fichiers

- Le serveur écoute sur le port `4021`
- Le client envoie un nom de fichier
- Le serveur retourne le contenu du fichier

> [!tip] Remarques sur l'envoi du contenu d'un fichier
> 1. Il faut préciser comment est envoyé le nom de fichier
> 2. Pour l'envoi du contenu, utiliser la redirection & recouvrement de processus

## Partie 2 : envoyer des fichiers

> [!tip] Rediriger le stdin vers le socket

### Envoyer le nom du fichier

**Problème :** Un nom de fichier est une chaîne de caractères terminée par le caractère spécial `\0` (en C).

La longueur de la chaîne n'est pas connue à priori par le serveur.

### Solution n°1

Voir [[#Problème de l'envoie de données de taille fixe|le problème de l'envoie de données de taille fixe]]

### Solution n°2

La taille variable à la taille de l'envoi dépend de la taille de la chaîne.

Idées : 
1. On coupe la connexion à la fin de l'envoi
   **+ Avantages :** Reste simple.
   **- Inconvénients :** ne permet pas de réutiliser la connexion.
2. Commencer par envoyer la taille de la chaîne, puis un second message avec la chaîne
   **+ Avantages :** simple.
   **- Inconvénients :** Il faut connaître la taille au moment de l'envoi (ce n'est pas un problème dans ce TP).
3. Marquer la fin de l'envoi par une donnée spéciale
   **+ Avantages :** Pas besoin de calculer la taille à l'avance.
   **- Inconvénients :** Plus complexe à la réception : détection du marqueur, allocation mémoire ; Choix du marqueur (ne doit pas apparaitre dans les données).



> [!tip] Il faut décider quoi faire du `\0` terminant la chaîne.
> Le client et le serveur doivent décider si la longueur de la chaîne inclue ou non `\0`



## Problème de l'envoi de données de taille fixe

Pour envoyer des données de taille fixe (par exemple, une information qui a toujours *100 octets*), on peut chercher :
- le marqueur de fin de chaîne (`\0`)
- Des valeurs quelconques pour arriver à 100 octets.

**Avantages :**
- Simple

**Inconvénients :**
1. Limite sur la taille de la chaîne (99 dans l'exemple).
2. Gaspillage de ressources si la taille est trop grande.
