---
title: Exceptions
draft: false
tags: 
aliases:
---
## Préambule

**Les exceptions offrent un moyen élégant et robuste de gérer les erreurs dans un programme.** Contrairement au mécanisme plus ancien des valeurs de retour en C, qui peut prêter à confusion et limiter les possibilités, les exceptions permettent de séparer clairement le code normal de celui qui traite les erreurs.

**Une exception est un objet** qui encapsule l'information relative à l'erreur survenue. Elle est instanciée et "lancée" (avec le mot-clé `throw`) lorsqu'une situation anormale est détectée. Le code appelant peut alors "capturer" cette exception dans un bloc `try/catch`, permettant ainsi de traiter l'erreur de manière appropriée.

**Le mécanisme est hiérarchisé** : toutes les classes d'exception héritent de la classe `Exception` (ou d'une de ses sous-classes). Chaque type d'exception possède un nom spécifique et un message d'erreur associé, facilitant ainsi l'identification et le traitement de l'erreur.

Par exemple :
```java
FileReader f = null;
try {
    f = new FileReader("hello.txt"); // si 'hello.txt' n'existe pas, un objet FileNotFoundException est lancé par FileReader()
    ...
}
catch(IOException e) { ... }
```

Dans l'exemple donné, on va notamment gérer les erreurs liées au système de fichier.
*Pour avoir plus de détails sur les `IOException`, voir [[Entrées sorties (I.O)#Principe général]]*

Aussi, si le fichier n'existe pas, l'exception `FileNotFoundException` va être renvoyée. Cette exception est héritée de `IOException`, ce qui nous permet d'obtenir les différentes erreurs possibles d'un coup. [Documentation](https://docs.oracle.com/en/java/javase/17/docs/api/java.base/java/io/FileNotFoundException.html)
## Les exceptions primaires, ou non vérifiées

Les exceptions primaires sont un ensemble de classes et sous-classes qui sont difficiles à prévoir et doivent être considéré comme des erreurs fatales. On le retrouvera particulièrement dans du code mal écrit qui effectue des opérations invalides sur la mémoire (par exemple, accéder au 8ᵉ élément d'une liste qui n'a que six éléments).

On peut les retrouver dans la classe `RuntimeException`, elle-même possédant des sous-classes tel que `NullPointerException`, `ArithmeticException`, ...

Ce sont des erreurs **non vérifiées** à la compilation

> [!tip] Gérer ces erreurs
> Si ce type d'erreur apparait, il est en général recommander de ne pas empêcher le programme de crash. Ca veut dire qu'il va falloir corriger le code en causes. 

## Les exceptions vérifiés

Les exceptions vérifiés sont, quant à elle, des erreurs probables, pas forcément critiques et qui dépendent du contexte. Elles sont encapsulées dans la classe `Exception`.

Une exception vérifiée doit être attrapée OU propagée. Si on ne fait pas une de ces deux actions, la compilation échouera.
### Attraper une exception vérifiée.

Pour "attraper" une erreur, on va alors utiliser un `try { .. } catch(..) { .. }`

**Syntaxe du `try-catch**`:
```java
try {
	// ...
} catch (<exception> e) {
	// ...
}
```
ou
```java
try (<expression>) {
	// ...
} catch(<exception> e) {
	// ...
}
```

> [!example]- Exemple de la seconde façon de faire
> De cette façon, on peut directement renvoyer l'erreur sans `catch`
> ```java
> public String lireFichier(String nomFichier) throws IOException {
> 	StringBuilder contenu = new StringBuilder();
> 	try (BufferedReader br = new BufferedReader(new FileReader(nomFichier))) {
> 		String ligne;
> 		while ((ligne = br.readLine()) != null) {
> 			contenu.append(ligne).append("\n");
> 		}
> 	}
> 	return contenu.toString();
> }
> ```


*Voir [[#Préambule]]

### Propager une exception vérifiée

Malgré tout, on pourrait ne pas **vouloir** gérer une erreur dans certaines méthodes, il faut alors la propager.

Pour ce faire, on va ajouter le code suivant à la fin de la signature de la fonction :

Par exemple :
```java
public String lireFichier(String nomFichier) throws IOException {
	StringBuilder contenu = new StringBuilder();
	FileReader fr = new FileReader(nomFichier);
	BufferedReader br = new BufferedReader(fr);
	String ligne;
	while ((ligne = br.readLine()) != null) { 
		contenu.append(ligne).append("\n");
	}
	return contenu.toString();
}
```

Ou encore, si on veut gérer le cas de conversion d'une chaine de caractère en nombre :
```java
String s1 = null;
String s2 = "salut";
System.out.println(s1.contains("a")); // génère NullPointerException car s1 = null
System.out.println(Integer.parseInt(s2)); // génère NumberFormatException
```

On ne capturera pas la première exception, car c'est une erreur **critique**, mais la deuxième peut (et c'est conseillé) être gérée.

> [!tip] Remarque
> Dans le cas de la méthode `lireFichier`, **partout** où je vais l'appeler, je dois, au choix :
> 1. Gérer l'erreur avec un **`try-catch`**
> 2. **Propager** cette même erreur
> Ce qui signifie que, si une méthode peut renvoyer une erreur, alors nous devront, à un moment ou un autre, gérer cette erreur.

### La propagation des erreurs non-vérifies

Imaginons une méthode `mth1()` qui peut renvoyer l'erreur non vérifiée `IndexOutOfBoundsException`. Si nous ne gérons pas l'erreur, elle sera propagée au code qui a appelé `mth1()`.

> [!fail]- Si l'exception atteint `main`
> Dans ce cas, si l'exception n'est pas arrêtée avant d'arriver au main et que, dans ce même `main`, on ne la contrôle pas, alors la JVM va tout simplement cesser crash.

*Voir [[#Le stack-trace]]*
## Le stacktrace

Un **stacktrace** (ou trace de pile en français) est une séquence d'appels de méthodes qui ont conduit à une exception dans un programme. C'est essentiellement un "journal" de l'exécution du programme à partir du point où l'erreur s'est produite, remontant jusqu'à la méthode d'entrée.

Nous avons vu précédemment qu'on pouvait propager une erreur.
J'ai pris l'exemple suivant :
```java
Exception in thread "main" java.lang.NullPointerException
        at com.example.myproject.Book.getTitle(Book.java:16)
        at com.example.myproject.Author.getBookTitles(Author.java:25)
        at com.example.myproject.Bootstrap.main(Bootstrap.java:14
```

On peut alors voir que l'erreur s'est propagée, partant de `Book.getTitle` (ligne 16), puis à `Author.getBookTitles` (ligne 25) puis à la méthode `Boostrap` (ligne 14), etc.

L'avantage des stacktrace est qu'on peut alors facilement retrouver l'origine de l'erreur.

> [!danger]- Avertissement
> Parfois, le stacktrace est un enfer dans d'autres langages. Voici un exemple avec le langage **Rust:**
> ![[Pasted image 20241001094738.png]]
> L'erreur vient de `event_receiver.rs` ligne 42, mais c'est beaucoup plus difficile d'extraire l'information.
> Heureusement, il n'y a pas ce problème aussi souvent sur Java, c'est même très rare


## Créer des exceptions customisées

Si aucune classe de l'API ne permet de représenter de façon pertinente la situation, il est alors possible de créer sa propre exception. Il suffit d'hériter de `RuntimeException`, `Exception` ou leurs enfants.

A ce moment, il est alors tout à fait possible de définir des attributs et méthodes, ou redéfinir les méthodes héritées.

> [!tip]- Dans la réalité
> Bien souvent, on va simplement appeler le super constructeur pour initialiser le message d'erreur.

**Exemple:**
```java
class PopulationException extends Exception {
	public PopulationException(){
		super("Problème avec la population");
	}
}
```

*Si besoin, on peut ajouter des attributs. On pourra également redéfinir la méthode `getMessage()` qui est héritée.*

**Second exemple:**
```java
class PopulationException extends Exception {
	private Population pop;

	public PopulationException(Population pop){
		super("Problème avec la population");
		this.pop = pop;
	}

	public String getMessage(){
		if (pop.taille() < 1) return "La population est vide";
		if (pop.onlyMen() || pop.onlyWomen()) return "La population ne peut plus grandir";
	}

}
```

> [!todo] Je n'ai pas terminé la suite

---
## TP


L'objectif est d'améliorer la gestion des erreurs lors des rencontres entre humains dans une simulation. Au lieu de renvoyer simplement `null` pour indiquer un échec, on va lever des exceptions spécifiques pour chaque type d'erreur possible lors d'une rencontre.

**Tâches à réaliser:**
1. **Création des classes d'exceptions :**
    - `BreedingForbiddenException` : Levée lorsque les deux individus sont du même sexe ou que les conditions physiques ne sont pas remplies.
    - `NoBreedingException` : Levée lorsque la fertilité ou le désir de procréer est nul.
    - `MeetingException` : Classe de base pour les deux précédentes, permettant d'avoir une hiérarchie d'exceptions.
2. **Modification des méthodes de rencontre :**
    - Les méthodes de rencontre dans les classes `Humain`, `Homme` et `Femme` doivent lever les exceptions appropriées en fonction des conditions.
    - Le message d'erreur de chaque exception doit être personnalisé pour indiquer clairement la raison de l'échec.
3. **Modification du moteur du jeu :**
    - Les appels aux méthodes de rencontre doivent être placés dans un bloc `try-catch` pour capturer les exceptions.
    - En cas d'exception, le message d'erreur contenu dans l'exception doit être affiché.
4. **Propagation des exceptions :**
    - La méthode `rencontre` de la classe `Population` doit propager les exceptions vers l'appelant. Cela permet de gérer les erreurs au niveau supérieur de l'application.

**Points clés:**
- **Hiérarchie des exceptions :** `MeetingException` est la classe de base, `BreedingForbiddenException` et `NoBreedingException` en héritent.
- **Messages d'erreur personnalisés :** Les messages doivent être clairs et informatifs pour faciliter le débogage.
- **Gestion des exceptions :** Utiliser `try-catch` pour capturer les exceptions et `throw` pour les propager.

**Cours de base:** [cours-info](https://cours-info.iut-bm.univ-fcomte.fr/index.php/menu-cours-s3/menu-mmi3-test/901-1-tp-nd2-polymorphisme-sp-487480037)