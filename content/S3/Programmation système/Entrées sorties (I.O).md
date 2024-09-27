---
title: Entrées sorties
draft: false
tags: 
aliases:
  - stdin
  - stdout
  - stderr
  - io
---
Les entrées/sorties (E/S ou **==I/O==**) s'appliquent sur:
- Les flux standards:
	- Entree standard (stdin, n°0)
	- Sortie standard (stdout, n°1)
	- Sortie d'erreur (stderr, n°2)
- Des fichiers en utilisant des objets appelés **descripteurs de fichiers**

![[diagram-stdin-stdout-stderr-702e578630d8d39c813d7d88c270c339.png]]

Exemple de redirection:
`ls 2> /dev/null`: permet de rediriger le flux `stdout` vers `/dev/null`.

Les entrées sorties peuvent se faire en utilisant soit les fonctions de haut-niveau ou bas-niveau.
## Principe général

Pour faire des entrées/sorties, on va:
1. Ouvrir le fichier (en lecture ou en écriture).
   **On obtient un descripteur**
2. Opérations de lectures et/ou d'écritures en utilisant le descripteur de fichier.
3. Fermer le descripteur de fichier

Les lectures/écritures peuvent se faire de manière formattées ou non.

> [!Error] **Indispensable**
> Il est **NECESSAIRE** de s'assurer que le fichier est bien écrit sur son support. Sinon des données vont être perdues/corrompues.
## APIs

### Fonctions haut-niveau

Ces fonctions sont fournies par la bibliothèque standard **libc**.
Elles sont indépendantes du système d'exploitation, c'est une interface accessible sur tout les systèmes courants.

**Libc** fourni également les **descripteurs de fichiers**, qui sont de type `File*`

### Bas niveau

A ce niveau, les fonctions sont fournies par le système et sont dépendantes de chaque système mais permettent des opérations plus spécifiques. (ex. permissions, pipelines...)


### Formattées vs non-formattées

Par formatté, on entend par là que les données ont été converties d'une représentation binaire en mémoire à une représentation texte dans le fichier.

Au contraire, non-formatté signifie que les données sont au format binaire.

**Exemple:**
La valeur 12 représentée en mémoire sur *un octet* sera présentée au format texte avec 2 caractères: '1' et '2'. 
On aura donc `00001100` en binaire, et `00110001 00110010` au format texte


> [!Summary|wide-3 min-0]+ Résumé
> Formatté = texte
> 
> Non-formatté = binaire
## Descripteurs de fichiers

> [!Warning] Work in progress

En bas niveau, descripteurs de fichiers = type int

### API Haut-niveau

**Documentation:** [page de man](https://man7.org/linux/man-pages/man3/fopen.3.html)

Au haut-niveau, on doit importer la librairie `stdio` de cette façon:
```c
#include <stdio.h>
```
*stdio = standard i/o*

Pour ouvrir un fichier:
```c
File* fopen (const char* name, const char* mode)
```

Avec les arguments suivants:
- **name**: nom du fichier à ouvrir
- **mode**: Le mode de lecture
	- `r`: lecture
	- `w`: écriture
	- `r+`: Lecture/écriture
	- `w+`: Lecture/écriture, Le fichier est tronqué. (Le fichier sera vidé)
	- `a`: écriture en ajout à la fin
	- `a+`: lecture/écriture en ajout

La fonction `fopen` renverra un **descripteur de fichier** ou la valeur **NULL** si il y a une erreur.

**Variantes:**
- `fdopen`
- `freopen`

**Fermer un fichier**

```c
int fclose(File* f)
```

Permet de fermer le descripteur donné.
Retournera `0` en cas de succès et `EOF` en cas d'échecs

Documentation sur EOF: [wikipedia.org](https://fr.wikipedia.org/wiki/End-of-file)

#### Exemple: Ouvrir un fichier 'toto.txt'

**En lecture:**

```c
File *f;
f = fopen("toto.txt", "r");

if (f == NULL) {
	perror("fopen");
	exit(-1);
}

// opérations de lectures...

if (fclose(f)) { // car si succès, alors = 0 donc false
	perror("fclose");
}
```

**En lecture/écriture:**

**Par octet:**
```c
// lecture
int getc(File* stream)
int fgetc(File* stream)
int getchar(void)

// écriture
int putc(int c, File* stream)
int fputc(int c, File* stream)
int putchar(int c);
```

**Par bloc:**
```c
size_t freed(void *ptr, site_t size, size_t nmemb, File* stream)
size_t fwrite(const void *ptr, size_t size, size_t nmemb,File* stream)
```

Lecture (écriture) de `nmemb` objets de taille `size` vers mémoire pointée par `ptr` depuis (vers) le descripteur `Stream`

**Exemple**
```c
// lecture
int bob[50];
size_t n;

n = fread(bob, sizeof(int), 50, f)
if (n != 50) { /* message */ }

// avec des char
char ligne[50]
size_t nl;

nl = fread(ligne, 1, 50, f) // char de taille '1'
if (n != 50) { /* message */ }
```

**Formatées**
```c
// sur stdout
int printf(cpnst char* format, ...)
// print sur le fichier de descripteur F
int fprintf(File* f, const char* format, ...)
// En mémoire à l'adresse pointée par S.
int sprintf(char* s, const char* format, ...)
// comme sprintf mais écrit au plus 'n' caractères
int snprintf(char* s, size_t n, const char* format, ...)
```

> [!Error] Méthode `sprintf` à éviter

```c
// Depuis stdin
int scanf(const char* format, ...)
// Depuis le fichier 'f'
int fscanf(File* f, const char* format, ...)
// Depuis la chaîne à l'adresse primaire pointée par 's'
int sscanf(const char* s, const char* format, ...)
```

#### Remarques TP

##### Exercice 1

**Consignes:**

Écrire une fonction :
```c
int copie(FILE *entree, FILE *sortie)
```
qui copie le contenu du fichier de descripteur entree dans le fichier de descripteur sortie. La fonction doit retourner le nombre d'octets copiés, ou -1 en cas d'erreur. La copie peut se faire par caractère avec getc() / putc() ou bien par bloc avec fread() / fwrite().

Tester cette fonction en écrivant un programme principal l'utilisant avec la copie de deux fichiers : un fichier texte (par exemple le code source du programme), et un fichier binaire (par exemple un fichier exécutable). Vous pourrez utiliser la commande cmp(1) pour vérifier que la copie est conforme à l'original.

**Code:**
```c
#include <stdio.h>
#include <stdlib.h>

// Retourne le nombre de caractères copiés ou -1 si erreur
int copie(FILE *entree, FILE *sortie) {
	int nb_copies = 0;
	int c = getc(entree);
	while (c != EOF){
		if (putc(c, sortie) == EOF)
			break; // erreur, putc a échoué
		c = getc(entree);
		nb_copies++;
	}

	if (ferror(entree) || ferror(sortie)) {
		return -1;
	} else return nb_copies;
}

int main(int argc, char *argv[]) {
	if (argc != 3) {
		fprint(stderr, "Usage: %s entree sortie\n", argv[0]);
		exit(1);
	}

	// arguments de la ligne de commande
	const char *fichier_entree = argv[1];
	const char *fichier_sortie = argv[2];

	FILE *entree = fopen(fichier_entree, "r");
	if (entree == NULL) {
		perror("fopen(entree)");
		exit(2);
	}
	FILE *sortie = fopen(fichier_sortie, "w");
	if (sortie == NULL) {
		perror("fopen(sortie)");
		exit(2);
	}

	int n = copie(entree, sortie);
	if (n >= 0){
		printf("Copie de %d caractère(s)\n", n);
	} else {
		printf("Erreur de copie\n");
	}

	// fermeture des fichiers
	if (fclose(sortie) != 0) {
		perror("fclose(sortie)")
	}
	if (fclose(entree) != 0) {
		perror("fclose(entree)");
	}
	return 0;
}
```
### Bas-niveau

L'interface est fournie par le système d'exploitation. Les descripteurs sont de type `int`.

En particulier:
- `0` pour l'entrée standard (STDIN_FILE)
- `1` pour la sorte (STDOUT_FILE)
- `2` pour la sortie d'erreur (STDERR_FILE)


## Pipelines

> [!Warning] TODO

