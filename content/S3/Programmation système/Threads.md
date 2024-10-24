---
title: Threads
draft: false
tags: 
aliases:
  - threads
---
## Principe

Les threads permettent d'avoir plusieurs fils d'exécution au sein d'un processus qui s'exécutent en même temps.

Les différents threads d'un processus **partagent les mêmes données**.

> [!danger] Les dangers de partager les mêmes données
> Si plusieurs threads utilisent les mêmes données, il faut alors mettre en place des protections (mécanismes d'exécution mutuelle par  exemple)
> 
> Pour plus d'informations, voir [[#Partage de données et la data-concurrence]]

## Intérêts des threads

Il y a de nombreux intérêts :
- Performance (si on a plusieurs processeurs) : Nous pouvons dans ce cas partager les tâches sur les différents cœurs du processeur.
- Organisation du programme
- Effectuer différentes opérations en simultanées
- etc.

## Termes et vocabulaires

 Concepts fondamentaux :
- **Thread :** Un fil d'exécution au sein d'un processus. Imaginez plusieurs tâches s'exécutant en parallèle dans un même programme.
- **Processus :** Un programme en cours d'exécution. Un processus peut contenir plusieurs threads.
- **Concurrence :** La capacité d'exécuter plusieurs tâches en même temps.
- **Parallélisme :** L'exécution simultanée de plusieurs tâches sur plusieurs cœurs de processeur (comme mentionné dans [[#Principe]]).
- **Deadlock :** Une situation où deux threads ou plus sont bloqués indéfiniment, chacun attendant qu'un autre libère une ressource. (*voir [[#Deadlock]]*)
- **Race condition :** Une situation où le résultat d'une opération dépend de l'ordre d'exécution des instructions de plusieurs threads. (*voir [[#Dataracing]]*)
- **Threads légers :** Des threads qui ont une empreinte mémoire plus faible et peuvent être créés en grand nombre. (*voir [[#'Green' threads]]*)
- **Mémoire partagée :** Une zone de mémoire accessible par tous les threads d'un processus.
- **Planificateur :** Le composant du système d'exploitation qui décide quel thread doit s'exécuter à un moment donné. (*voir [[#Threads systèmes]]*)


***


## Mise en œuvre (C)

En C, la bibliothèque `pthread` (POSIX threads) peut être utilisée
### Créer et démarrer un nouveau thread

```c
#include <pthread.h>

int pthread_create(pthread_t *thread,
				   pthread_attr_t *attr,
				   void *(*start_routine)(void*),
				   void *arg)
```

**Arguments de la fonction :**
- **`start_routine`:** `(function (void v) -> void*)`
- **`thread`**: L'identifiant du thread créé est enregistré dans `*thread`.
- **`attr`:** permet de passer des options pour la création du thread (NULL pour les valeurs par défaut)

### Attendre un thread

On peut attendre la fin d'exécution du thread en utilisant la fonction :
```c
int pthread_join(pthread_t thread, void** retval)
```
Où `thread` est l'identifiant du thread à attendre, et `retval` permet de récupérer sa valeur de retour.
### Terminer un thread

Le thread se termine à la fin de sa fonction, ou bien en utilisant la fonction :
```c
void pthread_exit(void* retval)
```
Où `retval` est la valeur retournée par le thread.

### Exemple

```c
#include <stdio.h>
#include <pthread.h>

// Fonction exécutée par le nouveau thread
void* task(void* arg) {
    int thread_id = *((int*)arg);
    printf("Hello from thread %d!\n", thread_id);
    pthread_exit(NULL);
}

int main() {
    pthread_t thread1, thread2;
    int thread_ids[2] = {1, 2};

    // Créer le premier thread
    pthread_create(&thread1, NULL, task, &thread_ids[0]);

    // Créer le deuxième thread
    pthread_create(&thread2, NULL, task, &thread_ids[1]);

    // Attendre la fin des threads (facultatif)
    pthread_join(thread1, NULL);
    pthread_join(thread2, NULL);

    printf("Main thread exiting.\n");
    return 0;
}
```

*Voir [[#Attendre un thread]] pour la fonction `pthread_join`*


***

## Mise en œuvre (Java)

*Ressource utile : [www.w3schools.com](https://www.w3schools.com/java/java_threads.asp)*

Pour utiliser des threads en Java, il faut :
1. Déclarer une classe qui hérite de `Thread`.
2. Implémenter la méthode `public void run()`, avec le code à exécuter pour le thread.

Afin de démarrer le thread, il faut appeler la méthode `start()`.
On peut attendre la fin du thread avec `join()`

**Exemple :**

```java
class Task implements Runnable {
    private int threadId;

    public Task(int threadId) {
        this.threadId = threadId;
    }
        
    @Override
    public void run() {
        System.out.println("Hello from thread " + threadId + "!");
    }
}
    
public class ThreadExample {
    public static void main(String[] args) {
        Thread thread1 = new Thread(new Task(1));
        Thread thread2 = new Thread(new Task(2));

        thread1.start();
        thread2.start();

        try {
            thread1.join();
            thread2.join();
        } catch (InterruptedException e) {
            e.printStackTrace();
        }

        System.out.println("Main thread exiting.");
    }
}
```


***

## Types de threads

### Threads systèmes

Les **threads systèmes** sont directement gérés par le système d'exploitation. Chaque thread système correspond à une entité d'ordonnancement distincte pour le noyau. Cela signifie que le système d'exploitation alloue des ressources (comme du temps CPU) à chaque thread de manière indépendante.

#### Exemple
Imaginez une voiture avec plusieurs passagers. Chaque passager représente un thread système. Le conducteur (le système d'exploitation) décide à quel moment chaque passager (thread) peut regarder par la fenêtre (utiliser le processeur). Le conducteur peut décider de changer de passager à tout moment, en fonction de différents critères (priorité, temps d'exécution restant, etc.).

#### Avantages

- **Flexibilité :** Grande flexibilité dans la gestion des threads, car le système d'exploitation offre un contrôle fin sur l'ordonnancement.
- **Performance :** Les threads systèmes peuvent tirer pleinement parti des capacités multicœur des processeurs modernes.

#### Inconvénients

- **Coût :** La création et la gestion de threads systèmes peuvent être coûteuses en termes de ressources système.
- **Complexité :** La programmation avec des threads systèmes peut être plus complexe, car il faut gérer explicitement la synchronisation et les communications entre les threads.

***

### 'Green' threads

Les **'green' threads** sont des threads gérés par un ordonnanceur utilisateur, c'est-à-dire un logiciel qui s'exécute au sein de l'application. Cet ordonnanceur décide lui-même quand passer d'un thread à l'autre, sans impliquer directement le système d'exploitation.

#### Exemple

Imaginez une troupe de théâtre où chaque acteur représente un 'green' thread. Le metteur en scène (l'ordonnanceur utilisateur) décide de l'ordre dans lequel les acteurs entrent en scène et de la durée de leur intervention. Le système d'exploitation (la salle de théâtre) ne s'occupe que de fournir la scène et les lumières, sans se soucier de la coordination des acteurs.

#### Avantages

- **Légèreté :** Les 'green' threads sont généralement plus légers à créer et à gérer que les threads systèmes, car ils ne nécessitent pas l'intervention du système d'exploitation à chaque commutation de contexte.
- **Flexibilité :** L'ordonnanceur utilisateur peut implémenter des stratégies d'ordonnancement spécifiques aux besoins de l'application.

#### Inconvénients

- **Performance :** Les 'green' threads peuvent être moins performants que les threads systèmes, car l'ordonnanceur utilisateur peut ne pas être aussi optimisé que celui du système d'exploitation.
- **Blocage :** Si un 'green' thread est bloqué (par exemple, en attendant une entrée/sortie), tous les autres 'green' threads du même processus peuvent être bloqués, parce que l'ordonnanceur utilisateur ne peut pas passer à un thread système.



***


## Partage de données et la data-concurrence

Partager les mêmes données entre plusieurs threads peut être TRÈS dangereux.

### Race condition

Une **race condition** survient lorsqu'un programme multithread accède à des données partagées de manière non atomique et que le résultat final dépend de l'ordre d'exécution imprévisible de ces accès. En d'autres termes, c'est une situation où deux ou plusieurs threads tentent de modifier une même donnée en même temps, et le résultat final dépend de l'ordre dans lequel ces modifications sont effectuées.

Imaginez un compteur partagé par deux threads. Chaque thread incrémente ce compteur plusieurs fois. Si les deux threads lisent la valeur actuelle du compteur en même temps, puis l'incrémentent, et enfin écrivent la nouvelle valeur, il est possible que l'une des incrémentations soit perdue. Par exemple, si les deux threads lisent la valeur 0, l'incrémentent et écrivent la valeur 1, le compteur final affichera 1 au lieu de 2, car l'une des incrémentations a été écrasée.

**Pourquoi c'est problématique ?**
- **Résultats imprévisibles :** Le comportement du programme devient non déterministe, c'est-à-dire qu'il peut produire des résultats différents à chaque exécution, même avec les mêmes entrées.
- **Difficulté de débogage :** Les race-conditions sont souvent difficiles à reproduire et à corriger, car elles dépendent de facteurs tels que le temps d'exécution et le planificateur du système.
- **Bugs subtils :** Les erreurs causées par les race-conditions peuvent être très subtiles et difficiles à détecter, surtout dans des programmes complexes.

#### Exemple

Imaginons un programme qui va ouvrir deux threads, le premier va ajouter 5 à `v` et le deuxième va ajouter 10 à `v`
On aura alors la suite d'exécution suivante :

| Thread 1                | Thread 2                |
| ----------------------- | ----------------------- |
| - lire la valeur de `v` | - lire la valeur de `v` |
| - Ajouter 5             | - Ajouter 10            |
| - Ecrire le résultat    | - Ecrire le résultat    |

Lorsque plusieurs threads accèdent à une même ressource partagée (ici, la variable `v`) et la modifient, il peut se produire des conditions de course. Ces conditions de course peuvent entraîner des résultats incorrects, des pertes de données, ou même des plantages de l'application.

***
### Deadlock

Un **deadlock** (impasse en français) est une situation dans laquelle deux ou plusieurs threads sont bloqués indéfiniment, chacun attendant qu'un autre libère une ressource qu'il détient. C'est une forme de blocage mutuel qui peut paralyser complètement une application multithread.

#### Exemple concret

Imaginez deux philosophes assis autour d'une table ronde, chacun avec une fourchette dans chaque main. Pour manger, un philosophe a besoin des deux fourchettes à côté de lui. Si chaque philosophe prend sa fourchette gauche, puis attend que sa fourchette droite se libère, aucun d'eux ne pourra jamais manger, car ils attendent tous que l'autre libère sa fourchette.

**Les quatre conditions nécessaires pour qu'un deadlock se produisent :**
1. **Exclusion mutuelle :** Une ressource ne peut être utilisée que par un seul thread à la fois.
2. **Attente circulaire :** Un ensemble de threads attendent chacun une ressource détenue par le thread suivant de l'ensemble.
3. **Non-privation :** Un thread ne peut pas se faire retirer une ressource qu'il détient.
4. **Attente :** Un thread doit attendre une ressource détenue par un autre thread.

#### Conséquences d'un deadlock:
- **Blocage de l'application :** L'application peut devenir complètement inutilisable si les threads bloqués sont essentiels à son fonctionnement.
- **Perte de ressources :** Les ressources détenues par les threads bloqués sont inutilisables jusqu'à ce que le deadlock soit résolu.
- **Difficulté de débogage :** Les deadlocks peuvent être difficiles à détecter et à reproduire, car ils dépendent souvent de l'ordre d'exécution des threads.

### Mécanismes de protection face aux deadlocks et aux race-conditions

> [!todo] Je dois m'en occuper, voici quelques ressources en attendant
> - https://stackoverflow.com/questions/34524/what-is-a-mutex
> - https://www.baeldung.com/cs/what-is-mutex

