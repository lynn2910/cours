---
title: Expressions lambda
draft: false
tags:
---
## Principe

**Les méthodes lambda sont intimement liées aux [[Interfaces]] en Java.** Elles sont apparues avec Java 8 pour simplifier la syntaxe et rendre le code plus concis, notamment lorsqu'il s'agit d'implémenter des interfaces fonctionnelles.

**Une interface fonctionnelle** est une interface qui ne déclare qu'une seule méthode abstraite. C'est le candidat idéal pour être implémenté par une expression lambda.

*Voir [[Interfaces#Les interfaces fonctionnelles]]*

**Une expression lambda** est une fonction anonyme qui peut être utilisée pour implémenter une méthode d'une interface fonctionnelle. Elle fournit une syntaxe plus concise et plus flexible que les classes anonymes traditionnelles.

Voici un exemple de déclaration d'une méthode lambda:
```java
Consumer<String> printer = (msg) -> System.out.println(msg);
```

Le passage d'une méthode comme paramètre d'une autre méthode (indirectement)
```java
public void mySort(List<T> list, Comparator<T> c) { ... }
```

> [!danger] L'emploi de conditions
> L'utilisation de conditions en instanciant des objets et en définissant la méthode abstraite peut être contre-productif concernant la concision du code
> Par exemple:
> ```java
> import java.util.function.*;
> class TestFuncInterface {
> 	public static void main(String[] args){
> 		Consumer\<Double> printer = val -> System.out.println("result: "+val)
> 		BiFunction<Integer, Integer, Double> div = (x, y) -> (double)x/(double)y;
> 		printer.accept(div.apply(3, 4));
> 	}
> }
> ```
> Dans cet exemple, il n'y aura aucune erreur, mais c'est illisible.

## Exemples
### Exemple n°1: classe nommée

```java
class Controller implements ActionListener {
	public void actionPerformed(ActionEvent event) {
		System.out.println("clic");
	}
}

class MyFrame extends JFrame {
	JButton but1 = new Jbutton(...);
	Controller c = new Controller();

	but1.addActionListener(c); // addActionListener prend un paramètre un ActionListener
}
```

### Exemple n°2 : classe anonyme

```java
class MyFrame extends JFrame {
	// ...
	JButton but1 = new JButton(...);
	but1.addActionListener(new ActionListener(){
		public void actionPerformed(ActionEvent event){
			System.out.println("click");
		}
	})
}
```

> [!tip] Cette méthode est moins lisible mais ressemble plus au Javascript
> *Voir [[Rappels Javascript#1. Fonctions anonymes]]*

### Exemple n°3: lambda

```java
class MyFrame extends JFrame {
	// ...
	JButton but1 = new Jbutton(...);

	but1.addEventListener(event -> { System.out.println("clic"); });
}
```

> [!success] Solution la plus lisible
>  Le compilateur comprend automatiquement que la méthode lambda est en réalité la méthode `actionPerformed` de l'interface `ActionListener`


## Notation fléchée

```java
(a) -> { return a+1; }
(x, y) -> { if (x>y) return x; return y; }
(String msg, int day) -> { System.out.println(msg + " " + day); }
() -> { System.out.println("hello"); }

() -> System.out.println("hello");
(a) -> -a
```

## Références de méthodes

S'il existe, dans l'API ou ailleurs, une méthode possédant la bonne signature et réalisant le traitement souhaité, pourquoi la redéfinir, fût-ce avec une fonction fléchée ?

Java permet de passer cette méthode par référence, qu'elle soit statique, d'instance ou de constructeur :
```
NomClasse::nomMethode
NomInstance::nomMethode
NomClasse::new
```

**Par exemple :**
- `System.out::println` équivaut à `x -> System.out.println(x)`
- `Math::pow` équivaut à `(x, y) -> Math.pow(x, y)`
- `MyClass::new` équivaut à `() -> new MyClass()`

