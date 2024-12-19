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

## Obtenir la largeur maximale

```java
public int nbNodesByLevel(Node n, int currentLevel, int searchLevel){
	if (currentLevel == searchLevel) return 1;

	int nb = 0;
	for (Node f: n.children){
		nb += nbNodesByLevel(f, currentLevel, searchLevel);
	}

	return nb;
}

public int maxWidth(Node n) {
	int max = 0, width = 0, level = 0;

	while ((width == nbNodesByLevel(n, 0, level)) > 0){
		if (width > max) max = width;
		level++;
	}
}
```

Mais cette méthode n'est pas efficace, on va plutôt l'implémenter de cette manière:
```java
public void countByLevel(Node n, int level, List<Integer> nbNodes){
	if (nbNodes.size() < level) nbNodes.add(1);
	else nbNodes.set(level, nbNodes.get(level) + 1);

	for (Node f: n.childre) countByLevel(f, level + 1, nbNodes);
}

```

# Arbre binaire ordonné

```java
class Node {
	Node left;
	Node right;
	int value;

	public Node(value){
		this.value = value;
		left = null;
		right = null;
	}
}
```

```java
public int insert(int value){
	insertRecur(root, value);
}

private void insertRecur(Node n, int value){
	if (n == null)
		root = new Node(value);

	else if (value <= n.value) {
		if (n.left != null)
			insertRecur()
	}
}
```