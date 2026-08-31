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

## HashMap

### Méthodes
| Méthode                 | Action                                                                                                              |
| ----------------------- | ------------------------------------------------------------------------------------------------------------------- |
| `V put(K key, V value)` | Ajoute/modifie un couple `(clé, valeur)`.<br>Si la clé existe déjà, l'ancienne valeur est remplacée par la nouvelle |
| `V get(Object key)`     | Renvoie l'objet associé à la clé `key`<br>si elle existe ou `null`                                                  |
| `V remove(Object key)`  | Supprimer la valeur associée à la clé                                                                               |
| `Set<K> keySet()`       | Renvoie un `Set` des clés                                                                                           |
### Utilisation.

```java
Map<String, A>  map1 = new HashMap<>();
Map<A, Integer> map2 = new HashMap<>();

A a1 = new A();
A a2 = new A();
Date d = new Date();

map1.put("toto", a1);
map1.put("toto", a2); // remplace a1 par a2
map1.put("a1", "toto"); // Erreur de compilation: pas le même type

map2.put(a1, new Integer(10));
map2.put(d, new Integer(5));

map2.containsKey(d); // renvoie false
map2.containsKey(a1); // renvoie true
map2.remove(d); // Ne fait rien
map1.remove("toto"); // Renvoie a2
map2.remove(a1); // Renvoie 10
```

## ArrayDeque

### File

| Méthodes             | Action                                                                                  |
| -------------------- | --------------------------------------------------------------------------------------- |
| `boolean offer(E a)` | Ajoute `e` en fin de queue                                                              |
| `E pull()`           | Supprime et renvoie l'élément en tête de queue.<br>Si la queue est vide, renvoie `null` |
### Pile

| Méthodes         | Action                                                                                                               |
| ---------------- | -------------------------------------------------------------------------------------------------------------------- |
| `void push(E a)` | Ajoute en tête de queue                                                                                              |
| `E pop()`        | Supprime en renvoie l'élément de tête de queue.<br>Contrairement à `pull`, provoque une erreur si la queue est vide. |
### Utilisation

```java
Queue<Double> q = new ArrayDeque<>();

q.offer(1);
q.offer(2);

int val = q.poll(); // renvoie '1'
```

