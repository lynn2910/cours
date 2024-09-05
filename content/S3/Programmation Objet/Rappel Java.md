---
title: Rappel JAVA
draft: false
tags:
---
## 1. Les classes

### 1.1 Vocabulaire

**Classe:** Modèle d'une chose/d'un concept. C'est un ensemble de propriétés/fonctionnalités.

**Attribut:** ==Une== des propriétés de la classe.

**Méthode:** ==Une== fonctionnalité de la classe.

**Membre (de classe):** Attribut ou méthode

**Objet:** Instance d'une classe. (Un exemplaire)

**L'instanciation:** Processus de construction d'un objet

**Constructeur:** Méthode particulière qui décrit comment créer un objet (initialiser les/des attributs).

**Destructeur:** Méthode particulière pour supprimer un Objet

> [!Warning] Constructeur & Destructeur
> Le cas général est plusieurs constructeurs et un seul destructeur (ex. C++), mais en Java, les destructeurs n'existent pas ([IBM garbage collector](https://www.ibm.com/topics/garbage-collection-java))

### 1.2 Déclaration/Définition

Le code Java sera dans les fichiers avec l'extension `.java`.

Les droits d'accès des membres sont spécifiés par:
- `public`: Tout objet peut y accéder, y compris sa classe
- `protected`: Tout objet de la même casse ou de ses sous-classes peut les manipuler. (S'étend également aux classes du package)
- `private`: Uniquement un objet de la classe peut le manipuler.

### 1.3 Instanciations

Instancier, c'est appeler un des constructeurs de la classe.


## L'héritage

### 2.1 Pourquoi? Comment?

L'héritage permet la modélisation d'un cas particulier d'une classe existante sans devoir ré-écrire tout ce qui leur est commun.

Par exemple, un Bus et une Voiture auront des points communs tout en étant différents en certains points.

### 2.2 Limiter l'héritage

Il est nécessaire de limiter l'héritage par la complexité (usage - profondeur). Elle ne concerne que les membres de statut Public ou Protected.

### 2.4 Surcharge/sur-définition

Avoir plusieurs méthodes qui portent le même nom est une surcharge de la méthode. Pour coexister, deux méthodes portant le même nom, doivent avoir une signature différente.

La signature est composée:
- Du nom
- Des types de paramètres (dans le sens donné)

Exemple de signature: `int add(int, int)`

#### Redéfinition d'attributs

> [!Warning] A éviter


```java
class A {
	protected int val;

	public A(int val){
		this.val = val;
	}

	public void print(){
		System.out.println(this.val);
	}
}

class B extends A {
	private int val;
	
	public B(int val){
		super(val);
		this.val = val;
	}

	public void print(){
		System.out.println(super.val);
	}
}

class C extends B {
	// ...plus accès à `val`
}
```

Dans ce cas, la classe `B` possédera deux propriétés `val` qui sont indiscernables.

#### Redéfinition de méthodes

La redéfinition de méthodes permet de remplacer le comportement d'une méthode héritée pour l'adapter la classe.

Cela permet de:
- Spécialiser le traitement lorsque celui de classe mère n'est pas assez spécifique
- La méthode héritée n'as pas forcément n'as pas nécessairement de définition. Il suffit qu'elle soit déclarés