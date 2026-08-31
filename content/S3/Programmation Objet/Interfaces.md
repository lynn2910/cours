---
title: Interfaces
draft: false
tags:
---
## Principe

En Java, une interface est un plan, un **contrat** qui définit **un ensemble** de méthodes que toute classe l'implémentant doit obligatoirement mettre en œuvre. Ce sont les méthodes abstraites, les éléments fondamentaux du contrat. Une classe peut implémenter plusieurs interfaces, étendant ainsi ses capacités sans être liée par un héritage unique. Depuis Java 8, les interfaces ont évolué, permettant désormais de définir des méthodes avec une implémentation par défaut et des méthodes statiques.

Les interfaces sont donc des outils puissants pour la programmation orientée objet en Java, favorisant la réutilisabilité du code, la modularité et le polymorphisme. Elles offrent un moyen flexible de définir des comportements communs à différentes classes, sans contraindre leur hiérarchie d'héritage.

## Exemple

Considérons l'interface `Triable` tel que :
```java
interface Triable {
	public int comparer(Triable t);
}
```

On peut alors faire :
```java
public void trier(List<Triable> List){
	boolean swap = true;
	int lim = list.size();
	Triable t1, t2;
	while (swap) {
		swap = false;
		for (int i = 0; i < lim - 1; i++) {
			t1 = list.get(i); t2 = list.get(2);
			if (t1.comparer(t2) > 0){
				list.set(i,   t2);
				list.set(i+1, t1);
			}
		}
		lim -= 1;
	}
}
```
*C'est l'algorithme [BubbleSort](https://en.wikipedia.org/wiki/Bubble_sort)*

Si on veut trier des bananes, on peut alors déclarer Banane tel que :
```java
class Banane implements Triable {
	protected int longueur;

	public Banane(int longueur){ this.longueur = longueur; }

	public int comparer(Triable t){
		if (t instanceof Banane){
			Banane b = (Banane) t;
			return (longueur - b.longueur);
		}
		return 0;
	}
}
```

*De nombreuses interfaces existent déjà pour toutes les collections et l'interface `Comparable` pour trier.*

## Les méthodes par défaut

> [!warning] Disponible avec Java8+

On peut définir des méthodes par défaut, telles que :
```java
interface Vehicle {
	// Méthode abstraite
	void start();

	// Méthode par défaut
	default void honk(){
		System.out.println("Beep! Beep!");
	}
}
```

On peut alors déclarer une class tel que :
```java
class Car implements Vehicle {
	@Override
	public void start(){
		System.out.println("Car started");
	}
}
```

On peut alors ne pas définir `honk` car ce n'est pas une méthode abstraite, son implémentation sera alors celle déclarée dans l'interface (le *default*).

Si on définit `Bike` tel que :
```java
class Bike implements Vehicle {
	@Override
	public void start(){
		System.out.println("Bike started");
	}
}
```

On peut alors écrire un code de ce style :
```java
Vehicle car  = new Car();
Vehicle bike = new Bike();

car.start(); // "Car started"
car.honk(); // "Beep! Beep!" car hérité de default

bike.start(); // "Bike started"
bike.honk(); // "Beep! Beep!" car hérité de default
```

## Méthodes statiques et privées

> [!warning] Les méthodes statiques dans les interfaces sont supportées depuis Java 8

> [!warning] Les méthodes privées dans les interfaces sont supportées depuis Java 9

```java
interface Vehicle {
	void start();
	static void info(){
		System.out.println("Interface vehicle");
	}

	private void helper(){
		System.out.println("Private helper method");
	}
}
```

## Les interfaces fonctionnelles

Les interfaces fonctionnelles possèdent plusieurs caractéristiques :
- `interface` ne comportant qu'une seule méthode abstraite
- Peut comporter d'autres méthodes : par défaut, privées et statiques.
- Il est recommandé d'annoter toute IF (interface fonctionnelle) avec `@FunctionalInterface`. Le compilateur pourra ainsi effectuer des vérifications utiles.
- Les IF s'utilisent principalement avec les **[[Expressions lambda]]** et les **Références de méthodes**

### Exemple

```java
@FunctionalInterface
interface Calculator {
	int calculate(int a, int b);

	default void printResult(int result){
		System.out.println("Result: " + result);
	}

	static void info(){
		System.out.println("This is a functional interface for calculation.");
	}
}
```

### Interfaces fonctionnelles natives

On retrouve plusieurs interfaces natives :
- Les `Function`: traitements acceptant un ou plusieurs paramètres et renvoyant un résultat. Elles déclarent des méthodes `T apply(..., ...)`
- Les `Consumer`: acceptent un ou plusieurs paramètres, mais ne renvoyant rien. Elles déclarent des méthodes du type `void accept(..., ...)`
- Les `Predicate`: acceptent un ou plusieurs paramètres et renvoient un booléen. Elles déclarent des méthodes du type `boolean test(..., ...)`
- Les `Supplier`: n'acceptent pas de paramètre, mais renvoient un résultat. Déclarent des méthodes du type `T get()`

Par exemple :
1. `BiFunction<T,U,R>` déclare `R apply(T t, R r)`
2. `IntToDoubleFunction` déclare `double applyAsDouble(int i)`
3. `DoubleConsumer` déclare `void accepte(Double d)`
4. `BiPredicate<T,U>` déclare `boolean test(T t, U u)`
5. `Supplier<T>` déclare `T get()`

> [!tip] Il existe beaucoup d'interfaces fonctionnelles intégrées dans Java.
> En général, l'interface va être réécrite plutôt que de passer des heures à chercher quelle interface convient.

### Avec les expressions Lambda

*Voir [[Expressions lambda]]*

Si on définit une interface `Doctor`
```java
@FunctionalInterface
public interface Doctor {
	void soigner(Humain h);

	default String examiner(Humain h){
		if (h.age > h.esperanceVie) return "mort";
		return "vivant";
	}

	default void soigner(Humain h){
		// ...
	}
}
```

Et une class `Population`
```java
class Population {
	List<Humain> pop;

	// ...

	public void traiter(Doctor doc){
		for (Humain h: pop){
			if ("vivant".equals(doc.examiner(h))) doc.soigner(h);
		}
	}
}
```

On peut alors écrire ce code :
```java
class Life {
	public static void main(String[] args){
		Population pop = new Population(...);

		Doctor badDoctor = h -> h.espereanceVie -= 10;
		pop.traiter(badDoctor);

		pop.traiter(h -> h.esperanceVie += 5);
	}
}
```

Cela nous permet de ne pas devoir définir une autre fonction tel que:
```java
void traiter(int t){ esperanceVie += t; }
```

> [!summary] Les interfaces fonctionnelles et méthodes lambda nous donnent de la souplesse

