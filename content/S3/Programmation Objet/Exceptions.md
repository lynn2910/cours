---
title: Exceptions
draft: false
tags: 
aliases:
---
## Préambule

**Les exceptions offrent un moyen élégant et robuste de gérer les erreurs dans un programme.** Contrairement au mécanisme plus ancien des valeurs de retour en C, qui peut prêter à confusion et limiter les possibilités, les exceptions permettent de séparer clairement le code normal de celui qui traite les erreurs.

**Une exception est un objet** qui encapsule l'information relative à l'erreur survenue. Elle est instanciée et "lancée" (avec le mot-clé `throw`) lorsque une situation anormale est détectée. Le code appelant peut alors "capturer" cette exception dans un bloc `try/catch`, permettant ainsi de traiter l'erreur de manière appropriée.

**Le mécanisme est hiérarchisé** : toutes les classes d'exception héritent de la classe `Exception` (ou d'une de ses sous-classes). Chaque type d'exception possède un nom spécifique et un message d'erreur associé, facilitant ainsi l'identification et le traitement de l'erreur.

Par exemple:
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

Aussi, si le fichier n'existe pas, l'exception `FileNotFoundException` va être renvoyer. Cette exception est héritée de `IOException`, ce qui nous permet d'obtenir les différentes erreurs possibles d'un coup. [Documentation](https://docs.oracle.com/en/java/javase/17/docs/api/java.base/java/io/FileNotFoundException.html)
## Les exceptions primaires, ou non-vérifiées

Les exceptions primaires sont un ensemble de classes et sous-classes qui sont difficiles à prévoir et doivent être considérer comme des erreurs fatal. On le retrouvera notamment dans du code mal écrit qui effectue des opérations invalides sur la mémoire (par exemple, accéder au 8e élément d'une liste qui n'as que 6 éléments).

On peut les retrouver dans la classe `RuntimeException`, elle-même possédant des sous-classes tel que `NullPointerException`, `ArithmeticException`, ...

Ce sont des erreurs **non-vérifiées** à la compilation

> [!tip] Gérer ces erreurs
> Si ce type d'erreur apparait, il est en général recommander de ne pas empêcher le programme de crash. Ca veut dire qu'il va falloir corriger le code en causes. 

## Les exceptions vérifies

Les exceptions vérifies sont, quand à elle, des erreurs probables, pas forcément critiques et qui dépendent du contexte. Elles sont encapsulées dans la classe `Exception`.

Une exception vérifiée doit être attrapée OU propagée. Si on ne fait pas une de ces deux actions, la compilation échoueras.]]*
### Attraper une exception vérifiée

Pour "attraper" une erreur, on va alors gérer utiliser un `try { .. } catch(..) { .. }`

**Syntaxe du try-catch**:
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

Pour ce faire, on va ajouter le code suivant à la fin de la signature de la fonction:

Par exemple:
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

Ou encore, si on veut gérer le cas de conversion d'une chaine de caractère en nombre:
```java
String s1 = null;
String s2 = "salut";
System.out.println(s1.contains("a")); // génère NullPointerException car s1 = null
System.out.println(Integer.parseInt(s2)); // génère NumberFormatException
```

On ne capturera pas la première exception car c'est une erreur **critique**, mais la deuxième peut (et c'est conseillé) être gérer.
## Le stack-trace

Un **stacktrace** (ou trace de pile en français) est une séquence d'appels de méthodes qui ont conduit à une exception dans un programme. C'est essentiellement un "journal" de l'exécution du programme à partir du point où l'erreur s'est produite, remontant jusqu'à la méthode d'entrée.

Nous avons vu précédemment qu'on pouvait propager une erreur.

J'ai pris l'exemple suivant:
```java
Exception in thread "main" java.lang.NullPointerException
        at com.example.myproject.Book.getTitle(Book.java:16)
        at com.example.myproject.Author.getBookTitles(Author.java:25)
        at com.example.myproject.Bootstrap.main(Bootstrap.java:14
```

On peut alors voir que l'erreur s'est propagée, partant de `Book.getTitle` (ligne 16), puis à `Author.getBookTitles` (ligne 25) puis à la méthode `Boostrap` (ligne 14), etc...

L'avantage des stack-trace est qu'on peut alors facilement retrouver l'origine de l'erreur.

> [!danger]- Avertissement
> Parfois, le stack-trace est un enfer dans d'autres langages. Voici un exemple avec le langage **Rust:**
> ![[Pasted image 20241001094738.png]]
> L'erreur vient de `event_receiver.rs` ligne 42, mais c'est beaucoup plus difficile d'extraire l'information.
> Heureusement, il n'y a pas ce problème aussi souvent sur Java, c'est même très rare





> [!todo] Je n'ai pas terminé la suite

1.2.2°/ Ecrire des méthodes qui génèrent des exceptions

- Comme dit en introduction, il est parfois pratique d'écrire des méthodes qui génère des exceptions (généralement vérifiées) pour signaler un cas d'erreur, plutôt que de renvoyer une valeur significative.
- Par exemple, si on écrit une classe pour gérer une liste d'objets, la méthode qui permet de mettre à jour un élément dans la liste peut tomber sur différents cas d'erreurs :
    - l'indice donné en paramètre est invalide (<0 ou > taille liste),
    - l'objet à stocker à cet indice est nul.
- La solution basique pour régler ces erreurs est de ne pas faire la mise à jour et renvoyer par exemple false pour indiquer que rien ne s'est passé.
- On peut également écrire la méthode afin qu'elle génère des exceptions. Dans ce cas, il faut que son entête spécifie quels types d'exception grâce au mot-clé throws (avec un s final)

Exemple :

```java
class Population {
    List<Humain> pop;
    ...
    public void setHumanAge(int index, int age) throws IndexOutOfBoundsException, PropertyVetoException {

        if (index <0 || index >= pop.size()) throw new IndexOutOfBoundsException();
        if (age <0 || age > 150) throw new PropertyVetoException();
        ...
    }
    ...
}
```

_Remarques_ :

- si ailleurs dans le code, on appelle setHumanAge(), il faudra obligatoirement mettre l'appel dans un try/catch pour capturer PropertyVetoException.
- en revanche, il n'est pas obligatoire de capturer IndexOutOfboundsException. Mais si on ne la capture pas, elle sera propagée implicitement, ce qui provoquera un arrêt de la JVM.

1.3°/ Propagation des exceptions

- C'est un mécanisme qui permet de "passer" une exception d'une méthode à une autre, soit implicitement pour les exceptions non vérifiées, soit explicitement pour les vérifiées.

1.3.1°/ Propagation implicite (= automatique)

- Par exemple, supposons que main() appelle meth1(), qui appelle meth2(), qui appelle meth3().
- Si meth3() génère un exception **non vérifiée** et que meth2() ne capture pas cette exception => meth2() va propager "automatiquement" l'exception à meth1().
- Si meth1() ne capture pas cette exception => meth1() va propager "automatiquement" l'exception à main().
- Si main() ne capture pas cette exception => la JVM arrête l'exécution.

1.3.2°/ Propagation explicite (= choisie par le programmeur)

- Par exemple, supposons que main() appelle meth1(), qui appelle meth2(), qui appelle meth3().
- Si meth3() génère un exception **vérifiée** et que meth2() ne capture pas cette exception => erreur de compilation.
- 2 solutions :
    - meth2() capture l'exception avec un try/catch
    - meth2() propage l'exception, en ajoutant dans son entête throws nom_exception. C'est donc le même principe que si meth2() génère elle-même l'exception (cf 1.2.2)
- Si meth2() propage l'exception, meth1() doit elle-même faire un try/catch ou propager l'exception à main().

1.4°/ Étendre les classes d'exception

- Si aucune classe de l'API ne représente de façon pertinente un cas d'erreur, on crée une nouvelle classe d'exception, soit en héritant de RuntimeException (ou ses sous-classes) si le cas d'erreur est critique, soit en héritant de Exception.
- Comme toute classe, on peut lui donner des attributs et des méthodes, et redéfinir les méthodes héritées.
- En pratique, dans une telle classe, on se contente souvent d'appeler le super constructeur pour initialiser le message d'erreur.

Exemple :

```java
class PopulationException extends Exception {
    public PopulationException() {
        super("This is a population exception");
    }
}
```

- Si besoin, on ajoute des attributs, initialisés via le constructeur, qui vont notamment être utiles pour redéfinir getMessage()

Exemple :

```java
class PopulationException extends Exception {
    Population pop;
    public PopulationException(Population pop) {
        super("This is a population exception");
        this.pop = pop;
    }
    public String getMessage() {
        if (pop.taille() == 0) return "population is empty";
        if (pop.onlyMen() || pop.onlyWomen()) return "population cannot grow";
    }
}
```

---

**2°/ Rencontre impossible = exception**

- Dans les premiers TP, quand deux humains h1 et h2 sont pris dans la population, la rencontre ne peut donner un bébé que si les deux humains sont de sexes opposés.
- Si ce n'est pas le cas, la méthode de rencontre renvoie un objet null.
- Pour bien faire, il vaudrait mieux générer une exception car c'est un cas d'erreur.
- Pour cela, créez le fichier BreedingForbiddenException.java à partir du code suivant :

```java
class BreedingForbiddenException extends Exception {
 
    protected Humain[] source;
 
    public BreedingForbiddenException(Humain h1, Humain h2) {
	super("naissance impossible : "+h1.getNom()+" et "+h2.getNom()+" sont de meme sexe");
	source = new Humain[2];
	source[0] = h1;
	source[1] = h2;
    }
 
    public Humain[] getHumain() {
	return source;
    }     
}
```

- Modifiez les classes Humain, Homme, Femme et le moteur du jeu afin d'utiliser l'exception définie ci-dessus lors de rencontres : s'il n'y a pas d'exception, on insère le bébé dans la population, sinon on affiche le message contenu dans l'exception.

**3°/ Rencontre non productive = exception**  

- Il est également possible que les deux humains soient de sexes opposés mais les conditions (sur le poids, age, ...) ne sont pas respectées ou bien le tirage aléatoire sur la fertilité a été négatif.
- Dans ces deux cas, il n'y a pas de nouvel humain et la valeur renvoyée est de nouveau null.
- Pour éviter ce problème, on va :
    - générer une BreedingForbiddenException quand les conditions sur le poids, age, ... ne sont pas respectées
    - générer une NoBreedingException quand le tirage sur la fertilité ou bien batifolage est négatif.
- Pour le deuxième cas, créez une classe NoBreedingException qui a comme attribut un Humain représentant la source de l'erreur et dont le message d'erreur est du type : "rencontre improductive : toto n'est pas fertile" ou bien "rencontre infertile : tutu veut batifoler"  
    

- Dans BreedingForbiddenException, modifiez le code pour avoir un message différent selon le cas (NB: cela impose de redéfinir la méthode getMessage()  ) :
    - même sexe : le message est le même que dans l'exercice 2
    - conditions d'âge et/ou poids non respectées : message donnant les conditions non conformes. Par exemple :  "naissance impossible : toto est trop jeune, tutu est trop vieille, tutu est trop gros"
- Modifiez Humain, Homme, Femme et le moteur de jeu pour utiliser les deux classes d'exception.

**4°/ Une super-classe pour les exceptions de rencontre.**

- Créez le fichier MeetingException.java à partir du code suivant :

```java
class MeetingException extends Exception {
 
    protected Humain[] source;
 
    public MeetingException(Humain h1, Humain h2) {
	super("problème de rencontre");
	source = new Humain[2];
	source[0] = h1;
	source[1] = h2;
    }
 
    public Humain[] getHumain() {
	return source;
    }
}
```

- Modifiez les deux classes d'exception pour qu'elles héritent de cette classe.

**5°/ la propagation des exceptions**

- Modifiez la classe Population en ajoutant une méthode rencontre :

```java
public Humain rencontre(int index1, int index2) {
   Humain h1 = getHumain(index1);
   Humain h2 = getHumain(index2);
   return h1.rencontre(h2);     
}
```

- Modifiez le moteur de jeu pour appeler cette méthode pour faire lors des rencontres, au lieu de le faire directement.
- On remarque que la méthode ne fait pas de try/catch. Or, elle génère potentiellement des exceptions. Le compilation va donc échouer.
- Modifiez le code ci-dessus pour que la méthode propage les exceptions explicitement au moteur de jeu.