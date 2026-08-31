---
title: Représenter des listes
draft: false
tags: 
aliases:
  - linked-list
  - LinkedList
---
## Listes chainées en Java

En Java, sans utiliser les listes de la librairie standard, il est possible de le faire avec le principe des **listes chainées**.

![[LLdrawio.png]]

On peut l'implémenter de cette façon :
```java
// Cell.java
public class Cell {
	public int value;
	public Cell next;

	public Cell(int v){
		this.value = v;
		this.next = null;
	}
}

// List.java
public class List {
	public Cell head;
	public int size;

	public List(){
		this.head = null;
		this.size = 0;
	}

	public Cell find(int value) {
		Cell c = this.head;
		while ((c != null) && (c.value != value)) {
			c = c.next;
		}
		return c;
	}

	public Cell get(int index){
		Cell c = this.head;
		int i = 0;
		while ((c != null) && (i < index)){
			c = c.next;
		}
		return c;
	}

	public Cell append(int value){
		Cell newCell = new Cell(value);
		
		if (size == 0) {
			head = newCell;
		} else {
			Cell c = get(sie - 1);
			c.next = newCell;
		}

		size++;
		return newCell;
	}

	public Cell insert(int value, int index){
		Cell c = null;
		Cell newCell = new Cell(value);
		
		if (index > size) index = size;
		if (size == 0) head = newCell;
		else if (index == 0){
			newCell.next = head;
			head = newCell;
		} else {
			c = get(index - 1);
			newCell.next = c.next;
			c.next = newCell;
		}
		
		size++;
		return newCell;
	}

	public Cell removeAt(int index){
		if ((index < 0) || (index >= size)) return null;

		Cell removed = null;
		if (index == 0){
			removed = head
			head = head.next;
		} else {
			Cell c = get(index - 1);
			removed = c.next
			c.next = c.next.next;
		}
		size -= 1;
		return removed;
	}

	// Version non-efficace
	public Cell remove(int value){
		Cell c = head;
		int i = 0;
		while ((c != null) && (c.value != value)){
			c = c.next;
			i++;
		}
		if (c != null) {
			c = removeAt(i);
		}
		return c;
	}

	// Version efficace
	public Cell remove(int value){
		Cell c = head;
		Cell p = null; // précédente
		while ((c != null) && (c.value != value)){
			p = c;
			c = c.next;
		}
		
		if (c != null){
			if (c == head){
				c = c.next;
			} else {
				p.next = c.next;
			}
			size--;
		}
		
		return c;
	}
}
```

> [!tip] Utiliser la généricité
> Il est possible d'utiliser la généricité, mais pour des raisons de simplicités, ça n'as pas été fait dans ce code.

## Listes chainées en C

**Définition de la structure de donnée :**
```c
typedef struct cell {
	       int   value;
	struct cell* next;
} Cell;
```

