La création d'un portfolio est un élément essentiel dans la recherche d'un travail, d'un stage ou d'une alternance.

## Pourquoi?

Le portfolio apporte plusieurs choses concrètes.

Tout d'abord, il permet aux recruteurs intéressés par le CV de mieux vous connaitre, au-delà d'une feuille format A4 bourrée d'informations et où tout a été compacté.

Un autre avantage quand vous avez **codé** votre portfolio, c'est que ca démontre vos compétences techniques, tant bien sur le développement web que la logique (par exemple un petit jeu en JS), mais aussi sur votre volonté;
Un portfolio fait-maison n'est pas quelque chose d'anodin, il faut le vouloir et aimer coder pour y parvenir.

## Les étapes cruciales

Dans la création d'un portfolio, il y a plusieurs étapes.

> [!Summary|wide-3 min-0]+ Note
> Chacun a ses méthodes de travail, je resterais donc général.

### Réflexion

En premier, on va se poser et chercher **ce qu'on veut**.
On va se demander si on parler de nos compétences, de nos projets, de nos parcours, si on veut mettre en avant un hobby, etc... Cette démarche est très importante et requiert du temps, car si vous ne prenez pas le temps d'y réfléchir, vous risquez de vous retrouver dans une situation où vous n'avez pas choisi les bonnes technologies/techniques, et parfois il sera nécessaire de recommencer... de zéro!

### Design (optionnel, mais conseillé)

Une fois les idées posées, il va falloir effectuer la démarche de designer son portfolio.

Mon conseil est de commencer par des croquis et notes sur comment l'organiser, puis établir les couleurs et le style général.
Aussi, il sera sans doute nécessaire de recommencer la maquette plusieurs fois pour atteindre ce que vous recherchez.

Prenons un exemple; Mes tentatives de portfolio.
#### Exemple

Voici ma première tentative:

![[Pasted image 20240830191639.png]]

Puis ma deuxième:

![[Pasted image 20240830191735.png]]

Puis la dernière tentative en date

![[Pasted image 20240830194442.png]]
*Cette maquette est encore en cours de création, elle va très certainement changée!*

On peut voir que le style des maquettes a joué au yoyo avec moi. D'un style minimaliste, j'ai tenté d'utiliser une palette de couleurs différente avec une page différente, puis je suis repassé sur une version plus minimaliste, tout en reprenant le *layout* de la 2e maquette.

Evidemment, chaque portfolio reflète la personne et est personnel, ca permet aussi aux recruteurs de savoir à qui ils ont à faire (les couleurs, le style général, le texte, le layout... Les recruteurs savent lire derrière ces informations)

> [!Info|wide-3 min-0] Note sur le responsive
> Le [responsive](https://developer.mozilla.org/fr/docs/Learn/CSS/CSS_layout/Responsive_Design) est un élément primordiale pour un portfolio; Les recruteurs verront peut-être votre portfolio sur mobile (depuis un QR code par exemple).
>  
> Je vous conseille de passer autant de temps sur la version ordinateur que sur la version mobile.

### Développement

Une fois le design établi, vient la partie la plus *marrante*: Le développement.

#### Déploiement

Tout d'abord, vous allez devoir vous demander comment vous comptez le déployer, et la raison pour laquelle c'est la première étape du développement est simple.

Si vous voulez ajouter un blog, il faudra une base de donnée.
Dans ce cas, des solutions tel que Vercel existent, ou alors vous pouvez l'héberger sur votre propre machine qui tourne 24h/24 7j/7.

Et si il n'y a pas besoin de base de donnée ou d'éléments **dynamiques** (pour faire simple, s'il ne faut pas de backend), le site peut alors être déployé sur des plateformes tel que **github pages** (il existe de nombreux services similaires).

Finalement, une étape optionnelle est de choisir son nom de domaine!
En effet, sur github pages (exemple), vous pourrez trouver **martindupont.github.io**, ce n'est pas le plus jolie et vous pourriez préférer un nom de domaine custom (utilisable également pour vos projets persos, et pas si chère).

*Martin* pourrait alors acheter le nom de domaine **martin.fr**. Ce ne serait pas mieux aux yeux des recruteurs?

Le meilleur dans l'histoire, c'est que les noms de domaines ce n'est pas si chère! (du moment qu'on ne prend pas un nom connu).

Par exemple, le nom de domaine **chamallow.fr** chez [OVHcloud](https://www.ovh.com/fr/order/webcloud/?#/webCloud/domain/select?selection=~()) coûteras 5.59€ puis 7.79€ **par an.**
Pourtant, un nom de domaine tel que **martin** coûtera + de 100€ au **minimum** (sauf **martin.christmas** bizarrement...). C'est comme vous voulez.

*Vous remarquerez que ce site est à l'adresse **lynn.chamallow.xyz**, c'est tout simplement le nom de domaine d'un amis qu'on se partage. On peut découper le nom de domaine **chamallow.xyz** en sous-domaines (avec leurs propres sous-domaines), utile pour des projets donc.*

#### Programmation

Une fois les choix liés au développement terminés, il est temps de commencer à programmer.

Encore une fois, il va falloir faire des choix.

React? Angular? VueJS? Astro? Flask? HTML Vanilla? 
CSS vanilla? SASS? Bootstrap? TailwindCSS? 

Il y a tant de choix que je ne peux tout simplement pas les lister et en conseiller un en particulier, car chacun a ses préférences!

Je préfère le combos VueJS + Less (CSS sous stéroïdes), mais un camarade préfèrera [AstroJS](https://astro.build/), et un autre préfère React + Tailwind.

Choisissez la techno que vous préféré et celle avec laquelle vous êtes le plus à l'aise, et si vous voulez découvrir une nouvelle technologie, **ne faites pas un portfolio!**
Ce sera une perte de temps. Faites une todo app, une horloge, un CRUD, un site sur les différentes races de chats (ok j'avais pas d'inspi) ou tout petit projet qui vous intéresse (ca fera aussi des projets à ajouter dans votre portfolio), mais un portfolio avec des technos que vous ne connaissez pas vous prendra juste plus de temps, quelques mots de têtes et des insultes envers la documentation ou le code qui ne marchera pas.