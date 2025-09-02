---
title: Client serveur
draft: false
tags:
---
# Définitions

**Client** : envoie des requêtes au serveur et attend le résultat.

**Serveur :** attend des requêtes, les traite, renvoie le résultat au client.

**Architecture client/serveur :**
- **bas niveau : (matérielle)** réseaux téléphoniques
- **haut niveau : (logicielle)** navigateur / serveur web

**Protocole :** codification des échanges entre client et serveur en termes de structure, contenu et ordonnancement dans le temps.

> [!example] Ethernet (bas niveau), http (haut niveau)

**Socket :** périphérique logiciel permettant l'envoi et la réception de suite d'octets vers/d'un autre socket.

**Domaine :** "contexte" d'utilisation du socket.
- pour communiquer entre deux processus d'une même machine = **domaine <u>UNIX</u>**;
- pour communiquer entre deux processus distants = **domaine <u>Internet</u>**.

**Protocoles associés :** TCP (mode connecté), UDP (mode non connecté), Raw, ...

**Implémentations client/serveur basé sur :**
- protocole d'envoi/réception de données : **TCP, UDP, ...**
- protocole orienté requête : RPC, RMI, HTTP

*Remarque :* La complexité réside dans la définition du protocole et son implémentation dans le langage choisi.

# Typologie client/serveur

**Mode de communication :**
- connecté : le client contacte (= se connecte) le serveur avant d'envoyer ses requêtes ;
- non connecté : le client envoie directement une requête au serveur, qui attend de n'importe qui. 

> [!example] Le mode *« non connecté »* est notamment utilisé dans le streaming de vidéos

**Durée de vie (Keep-Alive) :**
- client : une ou plusieurs requêtes, puis terminaison ;
- serveur exécuté en ***démon*** : sauf problème, tant que la machine fonctionne ;
- serveur exécuté ***à la demande*** (pax ex. via *inetd*) : terminaison quand aucun client.

**Possibilité d'interaction :**
- mono-requête : un seul type de requête possible (serveur horaire, echo, ...)
- multi-requêtes : plusieurs types de requêtes ;
- serveur mono-client : un seul client à la fois ;
- serveur multi-client itératif : plusieurs clients à tour de rôle ;
- serveur multi-threadé : plusieurs clients en parallèle.

*Remarque :*
- cas particuliers d'interactions serveur/serveur et/ou client/client (par ex. applications pair à pair (*peer to peer* ou *P2P*)).

# Socket

## Principe d'utilisation

### Client

1. Création socket ;
2. Liaison à une IP + port ;
3. demande de connexion à l'IP + port du socket serveur ;
4. envoi et réception de données sous forme de suite d'octets.

### Serveur 

1. Création d'un socket de connexion ;
2. liaison à une IP + port ;
3. attente de connexion ;
4. connexion acceptée → création automatique d'un socket de communication ;
5. envoi et réception de données sous forme de suite d'octets.

## 127.0.0.1 / localhost

C'est une interface réseau "boucle locale" associée à l'IP `157.0.0.1` ou `localhost`. Pas besoin de tester les applications avec 2 machines.

## En Java

Encapsulation dans des classes simples à utiliser (contrairement au C) :
- liaison automatique à l'IP + port fournies en paramètres ;
- connexion implicite au serveur ;
- ...

**Mode connecté :** classes `Socket` et `ServerSocket`

**Mode non connecté :** classes `DatagramSocket` et `DatagramPacket`

*Remarque :* pas de socket UNIX en Java.

> [!important] Données transmises en utilisant les classes de flux

### Mode connecté


```java
import java.net.*;
import java.io.*;

public class TcpServer {
	public static void main(String[] args) {
		int portServ = Integer.parseInt(args[0]);
		ServerSocket waitClient = null;
		Socket sockComm = null;
		
		try {
			waitClient = new ServerSocket(portServ);
			while (true) {
				sockComm = waitClient.accept();
				// ... communication avec le client
			}
		} catch(IoException e) {
			System.out.println("pb conEtxion client : " + e.toString());
			System.exit(1);
		}
	}
}
```

Et **coté client**

```java
import java.net.*;
import java.io.*;

public class TcpClient {
	public static void main(String[] args){
		String ipServ = args[0];
		int portServ = Integer.parseInt(args[1]);
		Socket sockComm = null;
		
		try {
			sockComm = new Socket(ipServ, portServ);
			// ... envoi et réception des données
		} catch(IoException e) {
			System.out.println("pb connexion client : " + e.toString());
			System.exit(1);
		}
	}
}
```

