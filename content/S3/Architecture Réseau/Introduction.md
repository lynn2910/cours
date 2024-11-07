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

