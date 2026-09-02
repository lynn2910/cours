---
title: Glossaire
draft: false
tags:
---

**Interface** : comporte ce qui est nécessaire pour demander un ensemble de services élémentaires. Elle indique précisément ce qu'un système peut faire sans donner d'information sur comment il le fait.

**Interface UML** : Une interface UML, en Java, C#, C++ etc. ne contient que des opérations. Pour demander un service à un objet, composant ou système, une opération est nécessaire et suffisante.

**Attributs :** Les attributs d'un objet permettent de stocker l'état interne de cet objet. Les attributs sont définis dans la classe de l'objet, et chaque objet a ses propres exemplaires en interne. L'encapsulation est la bonne pratique qui rend les attributs inaccessibles en dehors de l'objet (plus précisément du code de sa classe).

**Message :** Un message est l'équivalent d'une opération, il permet de demander un service à un autre objet (ex. `01 -> getName() -> 02`), avec d'éventuels paramètres. (Le concept de message existe en UML, mais pas en Java).

**Méthode :** Une méthode est l'implémentation d'une opération (en général du code). Les langages à objets permettent d'avoir 0, 1 et plus d'implémentations pour une opération. **Lors du cours, le terme "opération" est employé**. 

**Classe concrète :** Une classe concrète a au moins une méthode par opération. Cette méthode peut être définie dans la classe ou dans une classe ancêtre.

**Classe abstraite :** Une classe abstraite est une classe non concrète.

