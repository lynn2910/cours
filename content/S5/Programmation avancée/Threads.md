---
title: Threads
draft: false
tags:
---
# Définition

> [!note] Définition
>  Un thread est un processus "léger" s'exécutant au sein d'un processus classique, dit "lourd".

## Avantages

- partage du processeur entre threads moins gourmand (mémoire, temps, ...) qu'entre processus ;
- partage implicite du même espace mémoire entre threads.

## Inconvénients

- partage mémoire = conflits d'accès à régler ;
- thread qui plante = application qui plante.

# Pourquoi les threads

La création d'un processus lourd est facile en C, mais très compliqué en Java.

En plus, la mémoire partagée entre processus lourds est compliquée, il vaut mieux utiliser des threads.

Les problèmes principaux sont :
- éviter qu'un thread modifie une donnée partagée pendant qu'un autre la lit = **exclusion mutuelle** ;
- dans certains cas, régler l'ordre d'accès à une donnée partagée = **attente sur condition**.

# Les threads en Java

Afin de créer un thread, il faut créer une classe héritant de `Thread` avec :
- un **constructeur** dont certains paramètres sont les objets partagés entre threads 
- une méthode `run()`, c'est le point d'entrée de l'exécution du thread ;
- si besoin, d'autres méthodes nécessaires à l'exécution du thread peuvent être ajoutées.

Pour utiliser cette classe :
- on instancie la classe
- on appel la méthode `start()` pour commencer l'exécution du thread.

## Exemple 1 : Thread simple

```java
public class ThreadInc extends Thread {
	public int id;
	public int incVal;
	
	public ThreadInc(int id, int incVal) {
		this.id = id;
		this.incVal = incVal;
	}
	
	public void run() {
		System.out.println("I am thread " + id);
		int val = id;
		for (int i = 0; i < 5; i++) {
			System.out.println(val + ", ");
			val += incVal;
		}
		System.out.println("I'm dead");
	}
}
```

```java
public class ThreadTestInc {
	public static void main(String[] args) {
		Threading t1, t2;
		t1 = new ThreadInc(1, 2);
		t2 = new ThreadInc(2, 5);
		t1.start();
		t2.start();
	}
}
```

## Exemple 2 : Thread avec incrémentation sur objet partagé

```java
public class Box {
	public int val;
	
	public Box() {
		val = 0;
	}
	
	public int get(){
		return val;
	}
	
	public void increment(int incVal){
		val += incVal;
		System.out.println("val = " + val);
	}
}
```

```java
public class ThreadIncShared extends Thread {
	public int id, incVal;
	public Box b;
	
	public ThreadIncShared(int id, int incVal, Box b) {
		this.id = id;
		this.incVal = incVal;
		this.b = b;
	}
	
	public void run() {
		System.out.println("I am thread " + id);
		int val;
		for (int i = 0; i < 5; i++) {
			val = b.get();
			System.out.println("T" + id + " - " + val);
			b.increment(incVal);
		}
		System.out.println("I'm dead");
	}
}
```

```java
public class ThreadTestIncShared {
	public static void main(String[] args) {
		Threading t1, t2;
		
		Box b = new Box();
		
		t1 = new ThreadInc(1, 2, b);
		t2 = new ThreadInc(2, 5, b);
		t1.start();
		t2.start();
	}
}
```

**Remarques :**
- on va avoir des sauts de valeurs, car chaque thread pense avoir une valeur, quand elle a en réalité déjà été modifiée.

# Problème d'accès concurrents

L'accès se faisant uniquement en lecture aux attributs d'un objet partage, il n'y a pas de problème.

Si l'accès est possible en écriture, c'est problématique, il est probable que l'application plante.

*Exemple :* un serveur peut envoyer et effacer des fichiers locaux, via des méthodes `sendFile()` et `eraseFile()` :
- le serveur est multi-client : il crée un thread pour chaque client connecté ;
- un client A demande l'envoi d'un fichier F, alors qu'un client B en demande l'effacement ;
- **si** `sendFile()` est interrompu par `eraseFile` : crash.

# Exclusion mutuelle

Pour régler le problème d'accès concurrents, il faut rendre les méthodes d'accès aux objets partagés non interruptibles (= sections critiques).

> [!note] Utilisation d'un mutex
> Verrou que l'on associe à des données d'un objet partagé

## Principe

- un thread ayant accès à l'objet peut (dé)verrouiller le mutex ;
- si le mutex est déjà verrouillé, tout autre thread désirant y accéder est **mis en attente**.

## En C++

- On déclare une variable de type **mutex**, accessible par chaque thread, proposant des fonctions `lock()` et `unlock()` ;
- quand un thread veut manipuler l'objet partagé via une séquence d'instructions :
	- le mutex est verrouillé avant la séquence ;
	- le mutex est déverrouillé après la séquence.

> [!tip] Remarque
> On crée plusieurs mutex quand les séquences manipulent différents objets partagés.

## En Java

> [!danger] En java, il n'y a pas de classe de type **Mutex**
> MAIS chaque objet contient un mutex caché.

On utilise alors le mot-clé `synchronized`.

2 solutions possibles :
- classe avec des méthodes `synchronized` → Le mutex utilisé est celui de l'instance de la classe ;
- `blocks synchronized` → Le mutex utilisé est un objet annexe, par exemple attribut de classe.

Le verrouillage n'est pas directement fait dans le code des threads, mais **dans celui des objets partagés**.

### Exemple n°1 

```java
public synchronized int get(){
	// ...
}

public synchronized void increment(int incVal) {
	// ...
}
```

Et ailleurs dans le code :
```java
Box b = new Box();

b.increment(2); // Verrouillage du mutex de `b`
```

**Principe :**
- si un thread entre dans une méthode `synchronized`, la JVM verrouille le mutex de l'objet contenant cette méthode
	- les autres threads ne peuvent plus exécuter le code des méthodes `synchronized` (**la même ou les autres**) et sont mis en attente
	- ils peuvent quand même exécuter des méthodes *normales*.

### Exemple n°2 :  blocs synchronized

```java
public class Box {
	public int val;
	private Object lock;
	
	public Box() {
		val = 0;
		lock = new Object();
	}
	
	public void increment(int incVal) {
		synchronized(lock) {
			val += incVal;
			System.out.println("after inc : " + val);
		}
	}
}
```

### Exemple 3 : Exclusion mutuelle


```java
public class Building {
	private WC wc1,wc2,wc3;
	
	public void inWC1() {
		checkPaper();
		synchronized(wc1) {
			useWC1();
		}
		washingHands();
	}
	
	public void inWC1() {
		checkPaper();
		synchronized(wc2) {
			useWC2();
		}
		washingHands();
	}
	
	// ...
}
```

# Problème d'ordonnancement

Même si un accès à la ressource partagée est protégé par un mutex, il n'y a aucune certitude sur l'ordre d'exécution.

Dans certains cas, un ordre non figé amène à :
- **interblocage :** les threads attendent mutuellement que l'un relâche l'accès à la ressource partage ;
- **famine :** un ou plusieurs threads n'ont quasi jamais accès à la ressource.

Selon l'application, il est nécessaire de fixer l'ordre (par exemple, jeu du type "chacun son tour").

