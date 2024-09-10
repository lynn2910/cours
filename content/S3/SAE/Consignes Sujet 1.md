---
title: Consignes
draft: false
tags: 
aliases:
  - site-evenementiel
---
## Préambule

L'objectif de ce sujet est de réaliser une application web "moderne" dans un contexte full-stack, c'est-à-dire en implémentant aussi bien la partie interface graphique utilisateur (= front) au sein d'un navigateur, que la partie serveur (= back) sous la forme d'une API REST couplé avec un serveur de BdD. Le côté front prend la forme d'une application dite SPA (Single Page Application) implémentée en vuejs, et l'API REST est faite en nodejs avec le module express (entre autres). Le rôle du serveur n'est pas de fournir des pages HTML comme dans une application basée sur php, mais juste des données que le navigateur va utiliser pour modifier le visuel de la fenêtre.

Ce sujet offre également l'occasion de se familiariser avec un processus de développement qui part d'une étape de conception relativement poussée, afin de pouvoir implémenter ensuite un maximum de chose en parallèle.

## 1. Contexte

Le cadre global du sujet de type 1 est une application Web permettant de gérer une manifestation (au travers d'un navigateur), comme par exemple un forum étudiant, un salon thématique, un festival de musique, etc. Les possibilités d'interaction sont définies par rapport aux droits de l'utilisateur, à savoir public, prestataire ou ou organisateur, les deux derniers nécessitant une authentification.

Globalement :

- Un utilisateur public a accès à la page principale de l'événement qui présente une description textuelle/image, le plan interactif (de la manifestation), ainsi que la liste des prestataires (par ex. des groupes de musique, buvette, ...). L'utilisateur peut ainsi accéder à l'espace "personnel" de chaque prestataire et, s'ils existent, utiliser les "services" proposés par ceux-ci.
- La notion de service doit se comprendre comme "un service rendu au travers de l'application web" et pas comme "un service assuré lors de la manifestation". Par exemple, pour un prestataire qui fait de la restauration, le fait de vendre de la nourriture ne peut pas être considéré comme un service de l'application web. En revanche, le fait de pouvoir réserver un repas grâce à l'application web peut être considéré comme un service valide pour celle-ci.
- Autres exemples :
    - dans un festival de musique, un prestataire de type groupe de musique propose des services du type : planning avec horaires de passage, livre d'or, vente de disque/goodies, etc.
    - dans un forum emploi, un prestataire de type entreprise propose des services du type : réservation d'une plage horaire pour entretien, remplir un formulaire de contact/CV, planning de présentation de l'entreprise, etc.
- un utilisateur prestataire peut gérer son espace personnel et créant/modifiant sa page de présentation et en créant/paramétrant des services (parmi ceux proposés par l'application).
- un utilisateur organisateur peut créer/modifier la présentation de l'événement, le plan interactif, les prestataires.

Cette application doit être réalisée selon des principes de conception actuels du Web, à savoir avec une partie visuelle côté navigateur (front-end) écrite en Javascript grâce au **framework vuejs**, une partie base de données implémentée grâce à un serveur de type **MySQL**, une partie serveur (back-end) écrite en **nodejs**, qui fait l'interface entre le navigateur et la BdD, sous la forme d'une **API REST** et qui sert également à envoyer le code javascript créé en vuejs au navigateur (=comme un serveur web).

Cette organisation permet au navigateur, au travers des interactions de l'utilisateur avec l'interface graphique de l'application, d'envoyer des requêtes au serveur API. En fonction du type de requête et des données associées (en principe des objets JSON) le serveur API va exécuter certaines instructions permettant de mettre à jour la BdD, et/ou d'en extraire des informations pour produire de nouvelles données envoyées au navigateur. Ce dernier les utilise pour mettre à jour l'interface graphique.

La structure de l'interface graphique et de la BdD ainsi que les requêtes possibles à l'API sont à déduire du cahier des charges donné en section 2.

|   |
|---|
|IMPORTANT !<br><br>Les attendus en fin de semestre 3 ne sont pas de fournir l'implémentation complète de toutes les fonctionnalités. C'est pourquoi, le développement s'étalera également sur le semestre 4  (qui est relativement court)<br><br>Pour les fonctionnalités communes à toutes les applications, la section 2.3 précise quel est le minimum attendu en fin de S3 et S4.|

## 2. Cahier des charges

### 2.1 Droits d'accès et informations utilisateur

- L'application est basée sur trois types minimaux d'utilisateurs : public, prestataire, organisateur. Il est cependant possible d'ajouter d'autres rôles, notamment celui d'utilisateur enregistré, ce qui permet de gérer des cas où il faut conserver des informations personnelles sur le public (par ex, achat d'un billet d'entrée nominatif)
- Seuls prestataires et organisateur nécessitent **forcément** une authentification, ce qui suppose d'avoir des informations en BdD pour chaque utilisateur prestataire et organisateur. NB : cela n'empêche pas certains services de demander des informations aux utilisateurs publics de l'application, qui elles seront stockées en BdD.
- Ces informations utilisateur prestataire/organisateur doivent au minimum contenir :
    - un login,
    - un mot de passe,
    - un mail de contact,
    - le droits d'accès.
- Les autres informations nécessaires sont dépendantes du type de manifestation, notamment pour les prestataires. Par exemple, le public doit pouvoir faire une recherche multi-critères pour chercher des prestataires (cf. section 2.2.1), ce qui suppose des informations diverses pour catégoriser les prestataires. Autre exemple, les prestataires peuvent avoir des besoins logistiques (près d'un point d'eau, prises électriques, tables, ...) qu'ils doivent renseigner afin que l'organisateur puissent les placer.

### 2.2 Structuration de l'application

#### 2.2.1 Page principale

- La page d'accueil de l'application doit afficher :
    - un texte+images présentant la manifestation,
    - une image "interactive" donnant le plan de la manifestation,
    - la liste des prestataires avec un formulaire de recherche pour chercher/trier selon différents critères,
    - un moyen de s'authentifier/se déconnecter
- L'image interactive doit clairement faire apparaître l'emplacement "géographique" de chaque prestataire de la manifestation et quand le curseur de souris survole cet emplacement, une info-bulle doit afficher les informations principales concernant le prestataire, dont un lien pour aller vers l'espace personnel du prestataire.
- La liste des prestataires doit également permettre d'aller vers leur espace personnel.

#### 2.2.2 Page prestataire

- L'apparence de cette page dépend du type d'utilisateur.
- Pour public, la page doit afficher :
    - un texte+image présentant le prestataire,
    - la liste des services proposés par le prestataire, permettant d'aller vers la page dédiée à ce service.
- Pour le prestataire, la page doit afficher un menu ainsi qu'une partie dont le contenu changera selon l'item choisi dans le menu, avec au minimum :
    - la modification du texte+images présentant le prestataire, grâce à un éditeur wysiwyg incorporable dans une appli. web (par exemple tinyMCE ou JCE),
    - l'activation/désactivation/suppression de services, en précisant s'ils sont accessibles au public ou juste au prestataire.
    - la configuration des services.
    - la modification des informations utilisateur (cf. section 2.1)
- D'autres fonctionnalités peuvent être ajoutées librement selon les besoins induits par le type de manifestation.
- NB : un organisateur authentifié ne peut pas aboutir à cette page prestataire.

Remarques sur les services :

- La liste des services qu'un prestataire peut activer est laissée libre. En pratique, la longueur de cette liste dépendra surtout de l'état d'avancement du développement de l'application. Se reporter à la section Conseils pour des idées possibles de service.
- Comme indiqué ci-dessus, certains services ne sont pas forcément accessibles au public. Par exemple, si un prestataire active un service de type comptage du public, il est évident que c'est le prestataire lui-même qui doit utiliser ce service et non pas les visiteurs. En revanche, pour un livre d'or, l'accès peut être public.
- Configurer un service signifie créer/modifier toutes les données associées à un service. Cela peut être très simple ou très compliqué. Par exemple, pour un service du type livre d'or, cela consiste simplement à pouvoir changer l'entête du livre, alors que pour un service de type vente de goodies, cela implique tout un système de saisie des informations sur les articles vendus (image, description, prix, stock etc). 

#### 2.2.3 Page organisateur

- Cette page est affichée dès lors qu'un utilisateur de type organisateur est authentifié.
- La page doit afficher un menu ainsi qu'une partie dont le contenu changera selon l'item choisi dans le menu, avec au minimum :
    - la modification du texte+images présentant la manifestation, grâce à un éditeur wysiwyg,
    - définir la carte interactive avec notamment l'association d'un emplacement à chaque prestataire de façon plus ou moins automatique (cf. section 2.3.2),
    - la création/modification des prestataires, c.a.d. les informations des utilisateurs prestataires,
    - choisir un prestataire pour accéder à son espace personnel avec comme possibilités :
        - la modification du texte+images présentant le prestataire, grâce à un éditeur wysiwyg,
        - l'activation/désactivation/suppression de services.
    - la visualisation, à priori sous forme graphique, de statistiques, créées à partir des données récoltées grâce aux services que les prestataires ont publiés (cf. section 2.3.3)

### 2.3 Fonctionnalités communes

##### 2.3.1 Authentification

- L'authentification doit se faire sur la base d'un couple login/mot de passe qui est saisi via un formulaire dans le front-end.
- Comme en S3, il n'y a pas de connexion à l'API demandée, la vérification de l'existence de ce couple doit se faire via la source de données locale.
- Si le couple est correct, cette vérification doit renvoyer les informations utilisateur (notamment son rôle) plus un identifiant "de session" (tiré aléatoirement), le tout étant sont stocké dans la mémoire locale du navigateur (soit dans un local storage, soit en mémoire applicative)
- Du côté API, une route doit permettre de recevoir un couple, de faire la même vérification et de renvoyer les informations utilisateur s'il existe.

- Pour le S4, le principe reste le même excepté que le front-end envoie réellement une requête à l'API et que celle-ci renvoie d'autres informations permettant de sécuriser les requêtes futures (jwt, id de session, ...)

#### 2.3.2 Sécurisation des accès à l'API

- Les bases d'une sécurisation des accès à une API reposent sur l'envoi à chaque requête vers l'API d'une information permettant à cette dernière de vérifier si l'utilisateur front-end est authentifié ou non, et selon le cas d'autoriser ou non le traitement de la requête.
- Cette information peut être envoyée via l'URL, le corps de la requête, les headers, des cookies, etc.

- Pour le S3, un principe de sécurisation "à moitié fonctionnel" est le minimum demandé, sachant qu'il doit tenir compte de la contrainte du front-end et de l'API non connectés, mais qu'il soit facilement remplaçable par une sécurisation forte. Par exemple :
    - Côté front-end :
        - chaque fonction qui mime l'envoi d'une requête à l'API en allant chercher les données dans la source locale doit commencer par vérifier si l'utilisateur est authentifié et quels sont ces droits.
        - cette vérification se base sur les informations reçues lors de l'authentification et stockées localement.
        - si les droits sont adéquats, la fonction renvoie les données demandées, sinon une erreur.
    - Côté API :
        - on suppose que le front-end envoie l'identifiant de session obtenu lors de l'authentification, via la partie query de l'URL, par exemple : [https://monapi.org/presta/1234?session=12abc45-953-cfb12](https://monapi.org/presta/1234?session=12abc45-953-cfb12)
        - on  crée un middleware qui doit être appelé avant chaque fonction de contrôle qui nécessite des droits pour être exécutée.
        - ce middleware se contente de vérifier si la variable session existe dans l'URL et dans ce cas, permet d'appeler le contrôleur. Sinon, il renvoie une erreur.

- Pour le S4, il est demandé d'implémenter une méthode sécurisée (session, jwt, ...) comme vu en cours, ce qui nécessite des ajustements mineurs sur le front-end mais bien plus de travail côté API et BdD pour mettre en place le stockage des jwt/session et les contrôleurs qui vont les utiliser pour vérifier la validité d'un accès.

#### 2.3.3 Carte interactive

- Cette fonctionnalité est sans doute le plus gros challenge à relever et la façon de la construire dépend également du type de manifestation.
- La méthode pour créer le visuel de la carte n'est pas imposée. Cependant, il est très fortement conseillé de créer une image vectorielle svg qui peut être intégrée dans une page et manipulée très facilement via du javascript.

- Pour gérer la carte interactive, il y a 2 sous-fonctionnalités à implémenter :
    1. afficher l'image et quand la souris survole un emplacement, une info-bulle apparaît. Si les informations de l'emplacement sont vides, alors l'info-bulle contient seulement un bouton permettant de déclencher la saisie des informations (nom de l'emplacement, "moyens logistiques" disponibles par ex, surface /volume disponible, nombre de prises, accès à l'eau, etc). Si les informations ont été saisies, l'info-bulle contient le nom de l'emplacement + le prestataire associé (si l'étape 2 a été faite) et un bouton pour activer la modification des informations.
    2. modifier les associations emplacement-prestataire (par ex, par échange, par réaffectation sur un emplacement libre, ...)

- Pour le S3, le point n°1 est le minimum demandé mais en tenant compte du fait qu'il faudra faire le point n°2. Il faut donc que les informations liées au placement ne soient pas statiques mais en BdD.
- Le point n°2 doit être fonctionnel au minimum en fin de S4.
- Il est bien entendu possible d'ajouter des fonctionnalités comme par exemple la gestion de plusieurs cartes pour des manifestation multi-sites, ou bien la visualisation d'itinéraires en sur-impression de la carte, etc.

#### 2.3.4 Statistiques

- La mise en place de cette fonctionnalité dépend totalement des services qui vont être publiés et des données récoltées par ces services.
- Il est donc indispensable de prévoir des services où il y ait des données intéressantes à analyser. Quelques exemples classiques :
    - service comptage des visiteurs => statistiques sur l'affluence selon le type de prestataire, l'heure, ...
    - service vente de goodies => statistiques sur le type d'article en fonction de différents critères (âge de l'acheteur, type de prestataire, ...)
    - service formulaire de recensement/satisfaction => statistiques sur ce qui plait au public en fonction de différent critères.

- Pour le S3, il est demandé de fournir au minimum un graphique statistique, basé sur les données d'un service.
- Pour le S4, il est demandé un deuxième graphique.
- Il est cependant possible de fournir plus, comme un graphique paramétrable dynamique (par ex. avec un filtre sur les données), des graphiques concernant différents services, voire des croisements entre les données des services.

#### 2.3.5 Interface de documentation et test de l'API

- Comme dit précédemment, les fonctionnalités de l'application sont accessibles grâce à l'interface graphique web côté navigateur, qui va envoyer des requêtes à l'API pour obtenir des données susceptibles de mettre à jour l'interface graphique.
- Chaque fonctionnalité va donc correspondre à un ensemble de requêtes possibles à l'API.
- Dans le cas d'un API REST, chaque requête est identifiée par le type de requête HTTP sous-jacent (GET, PUT, POST, ...), une route (c'est à dire un chemin d'accès à des données, par ex, /users/getbyid/123), et éventuellement, des données transmises conjointement (souvent sous la forme d'un objet JSON)

- Une fois que toutes les fonctionnalités de l'application sont clairement définies, il est possible de définir toutes les requêtes et donc les routes associées.

- Conjointement à l'application, il est demandé d'utiliser **Swagger**, qui permet de générer une interface graphique de documentation et de test de l'API,
- Cela permet de tester facilement si une route est fonctionnelle dans l'API, si elle traite correctement les données entrantes et renvoie un résultat correct.

_Remarque_ : chaque route correspondant en gros à une fonction javascript qui va traiter une requête, il n'y a pas besoin que cette fonction soit pleinement opérationnelle pour être testée via l'interface de test. Cette fonction peut très bien dans un premier temps simplement renvoyer des données fixes mais au format attendu, et quand la BdD est opérationnelle, modifier son code pour accéder à la BdD.

- Pour le S3, il est demandé que toutes les routes déjà implémentées dans l'API soient documentées via swagger.
- Pour le S4, il suffira d'ajouter les routes nouvellement implémentées.

#### 2.3.6 Langue de l'application

- les pages publiques ainsi que celles des prestataires doivent être écrites en français ET en anglais, avec un moyen simple de changer entre les deux.

## 3. Conseils

- Afin que l'application ait un intérêt réel, il est conseillé de partir sur 4 ou 5 services que les prestataires peuvent activer (ou non). Il est également conseillé de choisir des services qui offrent plus ou moins de difficulté à être implémentés, aussi bien au niveau code que modèle de BdD.
- La liste ci-dessous donne quelques exemples possibles avec un degré de difficulté approximatif.
    - livre d'or (=facile)
    - formulaire de recensement ou satisfaction (comme on fait pour les JPO) (=facile)
    - comptage de visiteurs (=facile)
    - demande de renseignements via un formulaire mail (=facile)
    - visualisation graphique de l'affluence (=moyen)
    - planning de démonstration, concert, conférence, ... (=moyen)
    - réservation d'une place pour la démo/concert/conférence, ... (=moyen)
    - vente d'articles/goodies (=difficile)
    - création automatique d'un planning pour la journée en fonction des activités que l'utilisateur veut faire (=difficile)
- Cette liste n'est pas restrictive et c'est en fonction du type de manifestation qu'il faut trouver les services possibles.

- Pour créer des svg correctement intégrables dans une page web, il existe beaucoup de tutoriels sur le Net.
- Pour faire cette intégration, il existe différentes solutions qui sont données ici : [https://developer.mozilla.org/fr/docs/Learn/HTML/Multimedia_and_embedding/Adding_vector_graphics_to_the_Web](https://developer.mozilla.org/fr/docs/Learn/HTML/Multimedia_and_embedding/Adding_vector_graphics_to_the_Web)
- Si vous optez pour la version "basique" de la carte interactive, le plus simple est de copier/coller le svg directement dans le HTML. Sinon, il faudra passer par des modules permettant de manipuler les svg en vuejs, comme par exemple vue-svg-loader.

- Pour les statistiques, il existe de nombreuses bibliothèques permettant de créer des graphiques en javascript, compatibles avec vuejs. Il suffit de chercher sur le web et de suivre les exemples.