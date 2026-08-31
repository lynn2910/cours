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

# Les protocoles de communication

## Définitions

**Protocole :** codification des échanges entre deux parties communicantes (par ex. client et serveur) en termes de structure, contenu et ordonnancement dans le temps.

> [!danger] **Problème**
> Du point de vue applicatif, l'échange nécessite la possibilité d'envoyer/recevoir physiquement

> [!success] Un protocole repose généralement sur les fonctionnalités proposées par un protocole

## Principes de création

1. Définition à partir des « services » que propose le serveur, chaque service étant associé à une requête.
2. Chaque requête provoque un ensemble de communications + traitements.
3. Le protocole, c'est définir la structure de ces communications et leur ordonnancement, sachant que :
	- la communication initiale doit (en principe) commencer par l'identifiant (unique) de la requête ;
	- les réponses de même type (par ex. erreur, accusé de réception…,) doivent toutes avoir la même structure ;
	- une requête doit toujours recevoir une réponse, même si c'est un simple accusé de réception.
### Les étapes

Étapes de définition d'un protocole :
1. décrire de façon fonctionnelle les requêtes ;
2. décrire de façon fonctionnelle l'ensemble des données ) communiquer (+ éventuellement les traitements) associés à chaque requête, ainsi que leur ordonnancement ;
3. décrire la structure des communications en tenant compte du fait qu'à un moment donné de son exécution, un serveur ne doit recevoir/envoyer qu'une seule structure possible.

### Problèmes 

- ne pas oublier les cas d'erreur (souvent nombreux) ;
- trouver le « bon » nombre de communications nécessaires ;
- dépendant du langage d'implémentation envisagé ;
- contraintes de performance ;
- possibilité d'extension du protocole ;
- etc.

### Exemple : protocole pour un serveur horaire

#### Étape 1. Description des requêtes

1. Récupérer l'étape courante en fonction du fuseau horaire
2. Envoyer l'heure courante

#### Étape 2. Description des données communiquées

##### Requête 1

- c → s : identifiant requête, fuseau horaire ;
- s → c : erreur/ok sur fuseau ;
- s → c : si pas d'erreur, résultat dans l'heure courante.

##### Requête 2

- c → s : identifiant requête, login, mot de passe, heure ;
- s → c : erreur/ok, sur login et/ou m.d.p et/ou heure

#### Étape 3. Description des structures

> [!tip] Remarques
> - tous les identifiants requête doivent être uniques et avoir le même format (par ex, un int, une chaîne de caractère, ...) ;
> - cohérence dans la structure des réponses de type erreur/accusé de réception, mais pas forcément pour les résultats ;
> - on décrit généralement les formats :
> 	- soit au format texte (le plus facile) ;
> 	- soit au format binaire, avec les types classiques du C (plus lourd et compliqué).

##### Requête 1 : « GETTIME fuseau » 

- **erreur :** « ERR nb params » ou « ERR fuseau invalide »  
- **ok :** « OK » 
- **résultat :** « heure:minute:seconde »

##### Requête 2 : « SETTIME h:m:s login password » 

- **erreur :** « ERR nb params » ou « ERR login » ou ...
- **ok :** « OK » 
#### Etape 3*. Description des structures, format binaire

##### Requête 1 : un int valant 1 (identifiant requête) + 1 octet (fuseau)

- **erreur/ok :** un octet valant 0 (ok), -1 (erreur nb params), -2 (fuseau invalide)
- **réponse :** trois octets dans l'ordre `h`, `m`, `s`

##### Requête 2 : un int valant 2 (id req.) + 2 tableaux d'octets se terminant par `\0` (login, m.d.p) + trois octets (heure, minutes, secondes)

- **erreur/ok :** un octet valant 0 (ok), -1 (erreur nb params), -2 (erreur login), -3 (erreur m.d.p), -4 (erreur heure)

# Jsp 