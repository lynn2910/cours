---
title: Généricité
draft: false
tags:
---
## Principe

Ce concept permet de créer des structures de données (classes) et des fonctions/méthodes capables de travailler avec des types génériques au lieu de types spécifiques prédéterminés.

Le code générique peut fonctionner avec n'importe quel type de données (entiers, flottants, objets, etc.) sans être réécrit pour chaque type.
Dans le code, certains types ne sont pas spécifiés à l'avance, mais seront déterminés à l'exécution.

Les paramètres de type permettent de définir une sorte de *moule* ou de *modèle* (template) qui pourra ensuite être utilisant avec différents types réels.

## Déclaration

Pour déclarer une classe générique :
```java
class NomDeClasse<liste types generiques>
```

La liste des types génériques contient les noms des types (classes), qui sont en général notés pr des lettres majuscules.

**Exemples :**

```java
class Class1<T> { .. }
```

```java
class Class2<X, Y, Z> { ... }
```

En pratique, la liste ne comprend le plus souvent qu'un seul type générique.

> [!warning] Attention
> En Java, contrairement au C++, les types génériques sont forcément des classes et pas des types fondamentaux
> 
> Les types déclarés dans l'entête d'une classe générique peuvent être utilisés pour déclarer des attributs, des paramètres ou des valeurs de retour de méthodes.

### Exemple d'implémentation

```java
class MaClasse<T,U,V> {
	T attr1;
	U attr2;
	double attr3;

	public MaClasse(T t, U u, double d) {
		attr1 = t; attr2 = u; attr3 = d;
	}

	V maMethode(T t) {
		V val;
		if (t == attr1) { ... } // -> problème éventuel
		...
		return val;
	}
}
```

> [!warning] Comparaison `t == attr1`
> Ce n'est pas forcément le meilleur moyen de comparer.
> Nous pouvons utiliser la méthode `.equals` mais on ne sait pas si elle est implémentée dans `T`

### Méthodes d'instanciations

#### Instanciation classique

Dans ce cas, on va déclarer le type, tel que, par exemple :
```java
ArrayList<String> l = new ArrayList<String>();
```

#### Instanciation raccourcie

On peut également dire au compilateur de prendre directement les types :
```java
ArrayList<String> l = new ArrayList<>();
```

####  Exemples

```java
MaClasse<Integer, String, Date> m;
Integer i = new Integer(5);
String  s = "Bonjour";
m = new MaClasse<Integer, String, Date>(i, s, 1.2); // instanciation, syntaxe classique
m = new MaClasse<>(i, s, 1.2); // Instanciation, syntaxe raccourcie
```

```java
MaClasse<int, String, Date> m; // erreur de compilation

MaClasse<Integer, String, Date> m; 

Date d;
d = m.maMethode(new Integer(6)); // OK
d = m.maMethode(new Calendar()); // Erreur de compilation

float f = m.maMethode(new Integer(10)); // Erreur de compilation
```

## Utiliser des classes génériques

Pour les classes englobantes des types fondamentaux, le compilateur peut faire le transtypage automatiquement.

```java
MaClasse<Integer, String, Boolean> m;

m = new MaClasse<>(5, "Bonjour", 1.2);
boolean b = new m.maMethode(3);
```

> [!danger] Utilisation des opérateurs arithmétiques
> Les opérateurs tels que `+`, `-`, `*`, `/`, `%` et tous les opérateurs logiques (`>`,  `<`, `==`) sont à  **EVITER** sauf si c'est spécifiquement déclaré.
> 
> On préfèrera utiliser des interfaces telles que `Comparable` pour obtenir des méthodes utilisables de façon sécurisée (ex. `.equals()`, `.isLess()`, `compareTo()`, ...)

## Exemple

On peut par exemple implémenter la structure de données `Node`:
```java
class Node<T> {
	private List<Node<T>> children;

	private T data;

	public Node(T data){
		this.data = data;
		children = new ArrayList<Node<T>>();
	}

	public T getData(){ return data; }
	public void setData(T data) { this.data = data; }

	public void addChildren(Node<T> child){
		children.addChild(child)
	}

	public Node<T> createChild(T data){
		Node<T> n = new Node<T>(data);

		this.addChildren(n);
		return n;
	}
}
```

```java
class Tree<T> {
	private Node<T> root;

	public Tree() { root = null; }

	public Node<T> createNode(T data, Node<T> parent){
		Node<T> n;
		
		if (parent == null){
			n = new Node<T>(data);
			if (root != null) {
				n.addChildren(root)
			}
			root = n;
		} else {
			n = parent.addChildren(data);
		}
		
		return n;
	}
}
```

> [!tip]- Conseil d'implémentation
> Il est général conseillé de documenter en direct son code, car avec des types génériques, on doit prendre en compte plus de cas. 
> 
> En documentant le code et les méthodes, cela permettra d'utiliser plus facilement la classe et ses méthodes plus tard.
