---
title: Les arbres
draft: false
tags:
---
# Nœud

Pour implémenter un Noeud en Java:
```java
public class Node {
	public int val;
	public List<Node> children;

	public Node(int val) {
		this.val = val;
		this.children = new ArrayList<>();
	}
}
```

# Type de parcours

## Parcours en "profondeur d'abord"

Prenons cet arbre :
![[Pasted image 20241206110346.png]]

On peut implémenter ce parcours avec ce pseudo code :
```
type_retour parcours(Node n)
	// Traitement 1/n
	pour chaque fils de n
		type_retour = parcours(fils)
		// Tests sur val (optionnel)
		si val == ...
		sinon ...
		fin si
	fin pour
	// Traitement (optionnel)
	retourne valeur_defaut
```

# Implémentation

## contains

```java
boolean contains(Node n, int val) {
	if (n.val === val)
		return true;

	for (Node fils: n.children){
		boolean rep = contains(fils, val);
		if (rep) return true;
	}
	return false;
}
```

## Obtenir la profondeur

Il existe deux façons de l'implémenter :

```java
int treeDepth(Node n, int level){
	if (n.children.size() < 1) return level;

	int maxDepth = 0;
	for (Node fils: n.children){
		int d = treeDepth(fils, level + 1);
		if (d > maxDepth) maxDepth = d;
	}
	return maxDepth;
}
```

Ou :

```java
int treeDepth(Node n){
	if (n.children.size() < 1)
		return 1;

	int maxDepth = 0;
	for (Node fils: n.children){
		int d = treeDepth(fils);
		if (d > maxDepth) maxDepth = d;
	}
	return maxDepth + 1;
}
```

## Parcours en profondeur d'abord

```java
public List<Node> getNodePerLevel(Node n, int level){
	List<Node> list = new ArrayList<>();
	if (level == 0) {
		list.add(n);
	} else if (level > 0){
		int current = 1;
		Node separ = new node(-9999);

		Queue<Node> queue = new ArrayQueue<>();
		for (Node f: n.children) {
			queue.offer(f);
		}
		queue.offer(separ);

		while (!queue.isEmpty()){
			Node p = queue.poll();
			
			if (p == separ){
				if (current == level)
					return list;
				else {
					currentLevel += 1;
					queue.offer(separ);
				}
			} else {
				if (current == level) {
					list.add(p);
				} else {
					for (Node f: p.children){
						queue.offer(f);
					}
				}
			}
			
		}
		// Fin du while
	}
	return list;
}
```
