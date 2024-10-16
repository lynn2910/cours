---
title: Processus
draft: false
tags: 
aliases:
  - process
---
> [!danger]- J'ai tenté de corriger le cours, mais certains mots que le prof a écrits au tableau sont illisibles
> En cas d'erreur, je veux bien en être informé pour corriger ça.

## Définition

Un processus est une instance dynamique d'un programme en cours d'exécution, qui se matérialise par une image mémoire évolutive.

**Programme ou fichier binaire :** Représentation statique du code :
- Résultat de la compilation, il contient les instructions à exécuter dans un ordre précis.
- Son contenu est immuable tant qu'il n'est pas modifié.
- Les variables sont déclarées, mais n'ont pas de valeur définie avant l'exécution.
- Les fichiers ne sont pas ouverts et ne contiennent pas de données spécifiques à une exécution.

**Processus :** Instance dynamique du programme :
- Chargement du fichier binaire en mémoire, allouant les ressources nécessaires à son exécution.
- **Initialisation des variables :** Les variables déclarées dans le programme reçoivent une valeur initiale (souvent une valeur par défaut).
- **Allocation dynamique de la mémoire :** Les tableaux, les structures de données et d'autres objets sont créés en mémoire à la demande.
- **Ouverture et manipulation de fichiers :** Les fichiers sont ouverts, lus, écrits et fermés au cours de l'exécution.
- **État évolutif :** L'état du processus change constamment au cours de son exécution, en fonction des instructions exécutées et des données traitées.
- **Variables et état :** Les variables stockent les valeurs intermédiaires des calculs et reflètent l'état du processus à un instant donné.
- **Nature dynamique :** Le processus est une entité dynamique qui naît, évolue et meurt au cours de l'exécution du programme.

> [!tip]- Image
> Si le fichier binaire est le plan d'une maison, le processus est la maison elle-même, avec ses occupants, ses meubles, et ses activités en cours. Le processus est un environnement en constante évolution où le code du programme s'exécute et manipule des données.

## État d'un processus

Durant son exécution, un processus peut-être dans un des trois états suivants : (principaux états, valables pour tous les OS)
- **Actif**: Un processeur lui a été attribué. Ce dernier exécute une partie de son code.
- **Activable :** Il est prêt à être exécuté, il dispose de toutes les ressources nécessaires hormis un processeur.
- **En attente/bloqué :** Un évènement extérieur, ou une ressource est nécessaire à son exécution

![[Pasted image 20241003103644.png]]


### L'ordonnancement des programmes/ressources

L'ordonnancement des ressources et des processus est une opération fondamentale dans les systèmes informatiques, particulièrement dans ceux gérant plusieurs tâches simultanément. Son objectif principal est d'optimiser l'utilisation des ressources disponibles, qu'il s'agisse du processeur, de la mémoire ou d'autres périphériques, afin de maximiser la performance globale du système et de répondre aux besoins de tous les utilisateurs ou processus.

En effet, un système informatique doit arbitrer entre les différentes demandes de ressources, car celles-ci sont limitées. L'ordonnancement permet de déterminer quel processus sera exécuté à un instant donné, pendant combien de temps, et dans quel ordre les différentes tâches seront traitées. Cette décision est cruciale, car elle influence directement la réactivité du système, le temps d'exécution des tâches et l'équité entre les différents utilisateurs.

Les algorithmes d'ordonnancement varient en complexité et en objectifs. Certains privilégient la minimisation du temps d'attente moyen, d'autres la maximisation du débit, ou encore la garantie de temps de réponse pour certaines tâches critiques. Le choix de l'algorithme dépend des caractéristiques du système, des types de tâches à exécuter et des contraintes spécifiques.

![[cor_exo5_rectif.png]]


### Transitions d'états

- **Actif → Activable :** Le processus cède temporairement le processeur, souvent en raison d'un partage du temps de calcul.
- **Actif → Bloqué :** Le processus est suspendu en attendant la réalisation d'une condition extérieure, comme la fin d'une opération d'[[Entrées sorties (I.O)]], la réception d'un [[#Signal]] ou l'obtention d'une ressource (mémoire).
- **Bloqué → Activable :** La condition d'attente du processus est satisfaite, il peut donc reprendre son exécution.


## Création de processus

La création d'un processus est une opération dynamique qui permet de générer une nouvelle instance d'un programme en cours d'exécution. Cette opération, généralement réalisée à l'aide d'une primitive système dédiée (comme `fork()` sous Linux).

À la création d'un processus, le système d'exploitation entreprend plusieurs actions essentielles.

Tout d'abord, il alloue dynamiquement une zone de mémoire pour stocker le code, les données et la pile du nouveau processus. Ensuite, il crée un descripteur de processus, une structure de données qui contient des informations cruciales telles que l'identifiant du processus, son état actuel, ses registres et les ressources qui lui sont allouées.

Ce descripteur sert à gérer le processus tout au long de son cycle de vie. Enfin, le système d'exploitation insère le processus dans une file d'attente, généralement appelée liste des processus prêts, afin qu'il puisse être sélectionné par l'ordonnanceur pour être exécuté sur le processeur.

## Terminaison

Il existe deux façons de terminer un programme : Son destin peut être de **s'éteindre paisiblement** après avoir exécuté son code, ou d'être **abruptement interrompu** en cas de problème ou de [[#Signal]].

Lorsqu'un processus termine son exécution, le système d'exploitation procède à un nettoyage systématique : les blocs de mémoire, les fichiers ouverts, les périphériques réservés et autres ressources sont libérés, rendant ces ressources disponibles pour d'autres processus. De plus, le descripteur de processus, qui contenait toutes les informations relatives au processus, est détruit, marquant ainsi sa disparition définitive du système.
### Signal

Un signal, dans le contexte des systèmes d'exploitation comme Linux, est une forme de communication asynchrone entre processus. C'est un peu comme une interruption logicielle qui vient perturber le flux normal d'exécution d'un processus pour signaler un événement particulier.
#### Utilité

Les signaux servent à :
- **Notifier un événement:** Par exemple, un signal peut indiquer à un processus qu'il a reçu une interruption clavier (Ctrl+C), qu'un fichier est devenu accessible ou qu'une erreur s'est produite.
- **Terminer un processus:** Certains signaux sont utilisés pour demander à un processus de se terminer de manière ordonnée ou de manière abrupte.
- **Suspendre ou reprendre un processus:** Les signaux peuvent être utilisés pour mettre un processus en pause ou pour le redémarrer.

#### Les principaux signaux

Voici une liste non exhaustive des principaux signaux :
- **SIGTERM:** Demande de terminaison normale.
- **SIGINT:** Interruption par le clavier (Ctrl+C).
- **SIGKILL:** Terminaison immédiate (ne peut pas être interceptée).
- **SIGSEGV:** Erreur de segmentation (accès mémoire illégal).
- **SIGABRT:** Appel à la fonction `abort()`.
- **SIGALRM:** Expiration d'une alarme.
- **SIGCHLD:** Terminaison d'un processus enfant.

#### La réaction du processus

Un processus peut :
- **Ignorer le signal:** Le signal n'a aucun effet.
- **Exécuter un gestionnaire de signal:** Le processus exécute une fonction spécifique pour traiter le signal.
- **Terminer:** Le processus s'arrête.

Les fonctions `signal()` et `sigaction()` permettent de définir le comportement d'un processus face à un signal donné. Par exemple, on peut utiliser ces fonctions pour créer un gestionnaire de signal qui sera exécuté lorsque le processus recevra un SIGINT ou d'autres signaux.

### Agir sur les processus

#### Tuer le process

Pour tuer un processus, on peut utiliser la méthode `kill`:
```c
int kill(pid_t pid, int sig);
```
## Arborescence des processus

Le noyau Linux crée une structure arborescente de processus, où chaque nœud représente un processus. Le processus racine, **init**, est le point de départ de cette arborescence. Chaque processus, à l'exception de **init**, est lié à un processus parent par son PPID, formant ainsi une relation parent-enfant entre les processus.

L'organisation hiérarchique des processus sous Linux permet de tracer la lignée de chaque processus, depuis sa création jusqu'à sa terminaison. Le PID unique identifie de manière univoque chaque processus dans cette arborescence, tandis que le PPID établit le lien de parenté avec le processus créateur.

Le PID et le PPID sont deux attributs fondamentaux de chaque processus sous Linux. Le PID sert à identifier de manière unique un processus, tandis que le PPID établit le lien de filiation avec le processus parent, permettant ainsi de reconstituer l'arborescence des processus.

## Caractéristiques d'un processus

### PID & PPID

```c
#include <unistd.h>

pid_t getpid(void); // Renvoie le PID du processus appelant
pid_t getppid(void); // Renvoie le PPID du processus appelant
```

### Propriétaire

Le propriétaire d'un processus est l'utilisateur qui a lancé le processus. Il existe deux notions de propriétaire :
- **Propriétaire réel (UID réel)** : C'est l'utilisateur qui a effectivement lancé le processus. Il détient les droits les plus élevés sur le processus.
- **Propriétaire effectif (UID effectif)** : C'est l'utilisateur dont les droits sont utilisés pour vérifier les autorisations d'accès aux ressources. Il peut être différent du propriétaire réel en cas d'utilisation de mécanismes de changement d'utilisateur (par exemple, `sudo`).


```c
#include <unistd.h>

uid_t getuid(void);  // Renvoie l'UID réel du processus
uid_t geteuid(void); // Renvoie l'UID effectif du processus
gid_t getgid(void);  // Renvoie le GID réel du processus
gid_t getegid(void); // Renvoie le GID effectif du processus
```

**L'égalité entre UID réel et effectif (et GID réel et effectif) n'est pas toujours une évidence.** En effet, le système d'exploitation Linux offre des mécanismes pour modifier temporairement ces identifiants, offrant ainsi une flexibilité accrue en termes de gestion des droits d'accès.

## Recouvrement de processus

**Fork :** Duplique un processus existant, créant ainsi un nouveau processus fils qui est une copie quasi-conforme du processus père.

**Système d'appel exec :** Permet de remplacer le code et les données d'un processus en cours d'exécution par ceux d'un nouveau programme.

**Caractéristiques conservées et modifiées lors d'un exec :**
- **Conservées :** PID du processus parent (PPID), UID, GID, descripteurs de fichiers (sauf ceux ouverts avec le flag `O_CLOEXEC`), signaux en attente.
- **Modifiées :** Code du processus, données, PID (le nouveau processus reçoit un nouveau PID), arguments de ligne de commande, variables d'environnement.

### La primitive de recouvrement : `execvr`
```c
#include <unistd.h>

int execve(const char* filename, char* const argv[], char* const envp[]);
```

**Fonctionnement :**
Charge et exécute le programme binaire spécifié par `filename`. Le processus appelant est remplacé par le nouveau processus. L'exécution commence à la fonction `main` du nouveau programme.

**Paramètres :**
- **filename :** Chemin absolu ou relatif vers le fichier binaire à exécuter.
- **argv :** Tableau de chaînes de caractères représentant les arguments de la ligne de commande. Le premier élément `argv[0]` est généralement le nom du programme. Le dernier élément doit être un pointeur nul.
- **envp :** Tableau de chaînes de caractères de la forme "nom=valeur" représentant les *variables d'environnement* du nouveau processus. Le dernier élément doit être un pointeur nul.


**Retour :**
- `-1` en cas d'échec
- En cas de succès, la fonction ne retourne pas ! (le code a été remplacé)

> [!summary]- Remarques
> La variable d'environnement [[Variables d'environnement#PATH]] n'est pas utilisée pour trouver l'exécutable. Il faut définir son chemin (absolu ou relatif).
> 
> Le fichier doit être exécutable par l'utilisateur (droit `x`).
> 
> Si le fichier possède le bit de prise d'identité (`set uid`), alors le propriétaire effectif du processus est le propriétaire du fichier.

**Exemple :**

Exécution de `ls -l` avec le nom de fichier passé en argument.

```c
#include <stdio.h>
#include <unistd.h>
#include <stdlib.h>

int main(int argc, char *argv[]) {
    if (argc != 2) {
        fprintf(stderr, "Usage: %s fichier\n", argv[0]);
        return 1;
    }

    char *arg[4];
    
    // Initialiser les arguments
    arg[0] = "ls";
    arg[1] = "-l";
    arg[2] = argv[1]; // Le nom du fichier
    arg[3] = NULL;

    // Exécution de 'ls -l <fichier>'
    if (execve("/bin/ls", arg, environ) == -1) {
        perror("execve");
        free(arg);
        return 1;
    }

    // Ce code ne sera jamais atteint si execve réussit
    free(arg);
    return 0;
}
```

**Variantes de `execve`:**
- avec un `p`: la variable `PATH` est utilisé pour trouver l'exécutable
- avec un `e`: On peut spécifier l'environnement.

| Commande | Signature                                                              | Utilisation                                     |
| -------- | ---------------------------------------------------------------------- | ----------------------------------------------- |
| execl    | `int execl(const char* path, const char* arg, ... /* (char*) NULL */)` | arguments passés par liste d'arguments          |
| execlp   | `int execlp(const char* file, const char* arg, ...)`                   |                                                 |
| execle   | ...                                                                    |                                                 |
| execv    | ...                                                                    | arguments passés avec un tableau (ou *vecteur*) |
| execvp   | ...                                                                    |                                                 |
| execvpe  | ...                                                                    |                                                 |

## Redirections

Les redirections permettent de rediriger les flux standards (*voir [[Entrées sorties (I.O)]]*) depuis/vers d'autres fichiers/flux.

Cela peut se faire de deux manières :
### Par ouverture d'un fichier en l'associant à un descripteur de haut-niveau

Avec :
```c
FILE* freopen(const char* path, const char* mode, FILE* stream);
```

Cette méthode permet d'ouvrir un fichier (comme avec [[Entrées sorties (I.O)#API Haut-niveau|fopen]]) mais nécessite la **fermeture** du descripteur `stream` donné (qui doit être compatible avec le mode d'ouverture : lecture ou écriture) **une fois les opérations sur le fichier terminées.**

**Exemple :**
```c
File* redir;

redir = freopen("/tmp/sortie.txt", "w", stdout);
if (redir == NULL){
	// Erreur
}

printf("Coucou");
```

> [!tip] Le descripteur `stream` est fermé si nécessaire

### En modifiant le descripteur bas-niveau

Cela se fait avec les primitives `dup()` ou `dup2()`

C'est la méthode la plus générale qui consiste à dupliquer un descripteur existant.
Cette méthode fonctionne quelle que soit la sortie du descripteur (fichier, [[Entrées sorties (I.O)#Pipelines|tubes (pipelines)]], socket réseau…,).

Le principe général est le suivant :
1. Création d'un descripteur avec `open()`
2. Fermeture du descripteur à rediriger
3. Duplication du descripteur valide avec `dup()`
4. Fermeture du descripteur utilisé pour la redirection.

> [!tip] Remarques
> 1. L'étape `1` est inutile si le descripteur existe déjà (ex. [[Entrées sorties (I.O)#Pipelines|tubes (pipelines)]], socket réseau...,)
> 2. L'étape `4` est facultative
> 3. Les étapes `2` et `3` peuvent être combinées en utilisant la méthode `dup2()`


**Primitives :**
```c
#include <unistd.h>

int dup(int oldfd);
int dup2(int oldfd, int newfd);
```

**dup:** Duplique le descripteur `oldfd` dans le premier descripteur disponible.

**dup2:** Duplique `oldfd` dans le descripteur `newfd` qui est fermé si besoin.

**Exemple:**
```c
int fd = creat("/tmp/sortie.txt", "0640"); // (1)
close(STDOUT_FILENO) // (2)
dup(fd); // 3
close(fd); // 4
```

### Remarques

Les redirections restent en place après un fopen/exec, (sauf si le fichier a été ouvert avec `O_CLOEXEC`). C'est par exemple utilisé par le shell.
