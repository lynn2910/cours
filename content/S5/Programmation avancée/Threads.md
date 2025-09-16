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

