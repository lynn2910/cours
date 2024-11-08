---
title: Introduction
draft: false
tags:
---
Dans la programmation client-serveur, il est en général requis d'écrire deux programmes : un serveur et un client.

## Vocabulaire

- **Ping:** envoyer un paquet à une machine distante pour savoir si elle existe

## TCP

Le protocole TCP est le plus courant. Dans ce protocole, chaque paquet de donnée va être envoyé **dans l'ordre** et **sans perte**. Le protocole **HTTP** est notamment basé sur TCP

> [!success] Note sur les paquets perdus
> Quand un paquet est perdu, le client ou serveur va alors notifier de cette perte et l'auteur renverra le paquet. Cela permet de garantir qu'il n'y a pas eu de pertes de données.

### C'est quoi TCP

**TCP, c'est un peu le chauffeur de bus des données sur internet.** Imaginons qu'on envoie un colis à un ami par la poste. Pour s'assurer que toutes les pièces arrivent bien et dans le bon ordre, on les emballe soigneusement et on demande un accusé de réception. TCP, c'est un peu ça pour les données sur internet. Il s'occupe de découper les gros fichiers en petits paquets, de les numéroter, et de s'assurer qu'ils arrivent tous à destination et dans le bon ordre.

Si IP est l'adresse de la maison, TCP est le livreur qui s'assure que le colis arrive à la bonne porte. IP, un autre protocole important, indique simplement où les données doivent aller. TCP, lui, prend le relais et gère la livraison proprement dite. Il vérifie que les paquets ne sont pas perdus en route, et s'il y a un problème, il demande à renvoyer le paquet manquant. Grâce à TCP, il est par exemple possible de télécharger un fichier sans erreurs.

### Établir une connexion TCP

1. La connexion est établie
2. Le client envoie une requête et attend la réponse du serveur
3. Le serveur reçoit la requête du client, construit sa réponse et envoie la réponse au client
4. Le client reçoit la réponse du serveur
5. La connexion est fermée (terminée)

---
## UDP

---
## Sockets

Les sockets sont l'équivalent de *[[Entrées sorties (I.O)#Descripteurs de fichiers|descripteurs de fichiers]]* et permettent d'interragir avec le **stream**. Le **stream** est le lien entre les deux machines, qui va permettre de lire et écrire des données.

