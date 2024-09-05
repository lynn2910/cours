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

### Bas-niveau

> [!Warning] TODO
## Pipelines

> [!Warning] TODO

