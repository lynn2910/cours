---
title: Les collections
draft: false
tags:
---
# Types de collections
## Set

Un **Set** est un ensemble d'objets **non indicé**, sans doublons.
## List

Une **List** est un ensemble d'objets **indicés**, éventuellement avec des doublons.
## Map

Un **Map** est un ensemble associatif d'objets, **non indicés**, étant associés à une clé.

> [!summary] Définition d'une clé
> Une clé est unique dans un Map, mais plusieurs clés peuvent être associés à un même objet.
## Queue

Une **Queue** est un ensemble d'objets **non indicés** avec un système d'accès FIFO/LIFO

> [!success] FIFO/LIFO
> FIFO = First In First Out
> LIFO = Last In First Out

---
# Généricité des collections

Les collections sont toutes génériques.
Par exemple :
```java
Set<Integer> set = new HashSet<>();
```

> [!bug] Ce code ne marche pas
> ```java
> Set<‎ int> set = new HashSet<‎ int>();
> ```
> Voir [[Généricité#Types autorisés|Généricité > Types autorisés]]

Pour plus de détails, voir le cours sur la [[Généricité]]

---
# Implémentations

## HashSet

### Méthodes

| Méthode                    | Action                                                          |
| -------------------------- | --------------------------------------------------------------- |
| `boolean add(T e)`         | Ajoute l'élément `e` de type `T`                                |
| `boolean contains(T e)`    | Renvoie `true` si l'objet `e` existe dans le **HashSet**        |
| `boolean remove(Object e)` | Supprime et renvoie l'objet `e` s'il existe dans le **HashSet** |

### Utilisation

```java
public class A {}
public class B extends A {}

Set<A> set = new HashSet<>();

set.add(new A());
set.add(new A());
set.add(new B());
```

> [!bug] Cette ligne provoquera une erreur de compilation
> ```java
> set.add(new Date());
> ```
> La class `Date` n'est pas enfant de `A`, elle ne peut donc pas être ajoutée à ce **Set**

## ArrayList

### Méthodes

| Méthode                       | Action                                                                                       |
| ----------------------------- | -------------------------------------------------------------------------------------------- |
| `boolean add(T e)`            | Ajoute en fin de **List**                                                                    |
| `boolean add(int index, T e)` | Insert à l'index `index`.<br>Renverra une erreur si l'index est `< 0` ou `> taille tableau`. |
| `T get(int index)`            | Renvoie l'élément à l'adresse `T`                                                            |
| `int indexOf(T e)`            | Renvoie l'index de l'élément `e`<br>ou `-1` s'il n'est pas présent dans la liste.            |
| `boolean remove(int index)`   | Supprimer l'élément à l'index `index`<br>et renvoie `true` si l'élément a été supprimé.      |
### Utilisation

```java
List<A> list = new ArrayList<>();

list.add(new A());
A b = new B();
list.add(b);
list.add(new Date()); // Erreur de compilation: La class `Date` n'est pas enfant de `A`
list.add(15, new A()); // Erreur d'exécution: la liste n'as pas une taille >= à 15

A aa = null;
aa = list.get(0);

int index = list.indexOf(b); // Renvoie `1`
```
