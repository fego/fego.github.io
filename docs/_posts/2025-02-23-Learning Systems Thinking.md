# Intro
Vous est-il arrivé de commencer une mission plein d'enthousiasme, heureux de rejoindre un projet que l'on vous a présenté comme si exaltant ?
Et que, hélas, au bout d'un mois vous vous retrouviez déjà découragé car confronté à un système dysfonctionnant de toutes parts ? 
Une organisation très hiérarchique qui freine les initiatives, des bugs dans tous les sens, un code spaghetti ? 
Bienvenue dans le monde de la pensée linéaire. 

Vous avez envie de trouver des solutions, faire bouger des lignes ? 
Alors le livre *[Learning Systems Thinking](https://learningsystemsthinking.com/)*, de [Diana Montalion](https://montalion.com/), est fait pour vous. 
Le livre propose une introduction à la pensée systémique, dans le cadre des systèmes informatiques principalement. 
Il m'a permis de relier entre elles des idées que j'ai exploré auparavant, et de découvrir de nombreux concepts. 
Ce livre transforme déjà ma façon d'aborder la complexité dans mon quotidien au travail. 
Dans cet article je vous partage quelques idées qui m'ont particulièrement frappées. 
Comme à chaque fois, faire cet exercice de partage est pour moi aussi une façon de tester mes connaissances, ma compréhension. 
C'est d'ailleurs une pratique recommandée dans le livre. 

Pour cet article je me dois de mettre un *disclaimer* : je découvre ce domaine, donc la probabilité que j'écrive une bêtise est plus forte que d'habitude :)

![Couverture]({{ site.url }}images/Learning Systems Thinking.jpg)

Dans de nombreuses situations au travail, mais aussi dans la vie de tous les jours, nous nous confrontons à la complexité. 
Elle peut nous déstabiliser si nous sommes démunis face à elle. 
Nous pouvons avoir peur, blâmer les autres quand nous ne comprenons pas. 
Sans savoir comment l'affronter, nous trouverons probablement des mauvaises réponses. 
Nous pourrons même créer ou aggraver les problèmes. 
Et si nous pensons parfois comprendre la situation, cela peut aussi être une illusion. 

# Système
Les situations sont complexes car nous évoluons dans des systèmes complexes. 
Mais qu'est-ce qu'un système ? 
La définition de [Donella Meadows](https://fr.wikipedia.org/wiki/Donella_Meadows), une papesse de la pensée systémique : 

> An interconnected set of elements that is coherently organized in a way that achieves something (a goal).  
> Un ensemble d'éléments interconnectés qui est organisé de manière cohérente afin d'atteindre quelque chose (un objectif).

Dans le cadre du développement logiciel, Diana Montalion propose la définition suivante : 

> A system is a group of interrelated hardware, software, people, organization(s), and other elements that interact and/or interdepend to serve a shared purpose.  
> Un système est un ensemble de matériels, logiciels, personnes, organisations et d'autres éléments interdépendants qui interagissent et/ou se complètent pour servir un objectif commun.

Les relations sont très importantes et sont souvent les oubliées dans les analyses. 
Et un système qui fonctionne bien est un système dont les relations entre les éléments sont harmonieuses.  
Les éléments du système fonctionnent comme un tout de façon naturelle, fluide. 
Cette harmonie du système est appelée "intégrité conceptuelle". 

Combien de fois avez-vous vu un tel système ? 

Moi, jamais. 
En 20 ans de vie de développeur. 

# La pensée linéaire
Prenons un exemple qui peut paraître simple. 
Un bug dans un système informatique. 
On va essayer de comprendre la ligne de code qui pose problème, et appliquer un correctif. 
Le problème sera réglé. 
Pour aujourd'hui. 
Et sans prendre plus de recul, il est fort probable que d'autres problèmes similaires apparaissent tôt ou tard. 
On a appliqué un pansement.  
L'approche du raisonnement sous l'angle purement logique, rationnel, en décomposant le tout en sous parties est dit "pensée linéaire". 

Elle est favorisée par notre société. 
En soit ce n'est pas une mauvais façon de pensée. 
La pensée linéaire est très utile dans de nombreux contextes. 

> We are taught to think linearly. Linear thinking is so ubiquitous, many of us don’t recognize it as one type of thinking. We call it, simply, thinking: predictable, rational, repeatable, procedural, dualistic, top-down, and concerned with control 
> -- Diana Montalion, Learning Systems Thinking  
> On nous apprend à penser de manière linéaire. La pensée linéaire est tellement omniprésente que beaucoup d'entre nous ne la reconnaissent pas comme un type de pensée. Nous l'appelons simplement la pensée : prévisible, rationnelle, reproductible, procédurale, dualiste, descendante et axée sur le contrôle 
> -- Diana Montalion, Learning Systems Thinking

Mais on est limité (souvent) par une vision axée principalement sur le profit, la rapidité (productivité). 
Tout est censé être sous contrôle, calculable, prédictible. 
On récompense ceux qui restent dans ce modèle de pensée, et on regarde de travers ceux qui font des pas de côté. 
L'échec est mal vu. 
Cela nous pousse à ne pas élargir notre pensée. 
Or une chose dont on peut être certain dans un système informatique de nos jours, c'est que rien n'est certain. 

Le chapitre sur la pensée linéaire commence par cette citation. 
Elle explique mieux que ce que je n'ai pu le faire les limites de la pensée linéaire : 
> [B]lack-and-white logic can sometimes work even for complex systems. But it ignores the ways in which parts interact with one another. Reductionism may serve to explain how a bird flies, but not how a flock of birds move in unison. It may describe internal combustion, but not traffic patterns. It may describe electric patterns in the brain, but not consciousness, and it’s unlikely that anyone or anything—not even the world’s most powerful computers— will ever fully analyze the interactions that make for healthy soil.
—Mark Bittman, Animal, Vegetable, Junk (Mariner)  
> La logique "tout blanc ou tout noir" peut parfois fonctionner, même pour des systèmes complexes. Mais elle ne tient pas compte de la manière dont les éléments interagissent les uns avec les autres. Le réductionnisme peut servir à expliquer comment un oiseau vole, mais pas comment une volée d'oiseaux se déplace à l'unisson. Il peut décrire la combustion interne, mais pas les schémas de circulation. Il peut décrire les schémas électriques du cerveau, mais pas la conscience, et il est peu probable que qui que ce soit ou quoi que ce soit - pas même les ordinateurs les plus puissants du monde - puisse un jour analyser complètement les interactions qui sont à l'origine d'un sol sain. --Mark Bittman, Animal, Vegetable, Junk (Mariner)

Si je reprends l'exemple du bug, voyons ce que nous ne voyons pas à cause d'une pensée purement linéaire. 
Nous ne prenons pas la hauteur nécessaire pour comprendre les mécanismes, les relations entre les éléments du système, qui ont permis l'apparition du bug. 
Quelques questions que nous pourrions nous poser : 
* Est-ce que nous avons interrogé les pratiques de test de l'équipe ? 
Une absence d'écriture de tests unitaires ? 
* Le bug était-il plutôt une mécompréhension du besoin, qui était mal formulé car écrit trop vite et non discuté par l'équipe ? 
Il y a-t-il des rituels d'échanges sur les *user stories* (par exemple un atelier d'*[example mapping](https://cucumber.io/blog/bdd/example-mapping-introduction/)*) ?  

Un bug peut être une opportunité de comprendre un dysfonctionnement plus profond. 
De trouver un point de bascule nous permettant d'agir plus en prodondeur, sur une cause racine. 

Limiter notre pensée à la pensée linéaire, c'est aller droit dans la mise en place d'un système bancal (sans intégrité conceptuelle). 

La solution ? 
Développer une pensée non linéaire, prenant en compte les relations entre les éléments d'un système. 

![pensée linéaire]({{ site.url }}images/linear_non_linear.png)  
Source : [Ted de Anna Justice](https://www.youtube.com/watch?v=bNASybOzruM)

On pourrait penser qu'en se libérant du cycle en V, typiquement linéaire (processus séquentiel, divisé en étapes prédictibles), les organisations pratiquent la pensée systémique. 
Mais nous faisons de l'agilité comme nous faisions du cycle en V. 
En essayant de toujours tout contrôler, simplifier. 

> Agile was invented because reality refuses to bow down to power -- Joe Eaton  
> L'agilité a été inventée parce que la réalité refuse de s'incliner devant le pouvoir -- Joe Eaton 

Il faut changer la culture plus en profondeur. 
Mais quelles sont les pratiques qui vont nous aider à embrasser la pensée systémique ?

# Les pratiques de la pensée systémique
## Apprendre
Apprendre, toujours. 
Ne jamais s'arrêter. 
Prendre un sujet qui  nous intéresse, nous motive, et que l'on peut pratiquer, et le creuser. 
J'ai abordé le sujet il y a quelque temps dans un [article]({{ site.url }}2020/09/27/Apprendre-à-apprendre.html). 

## Structurer sa penser
Il faut être capable de structurer sa pensée pour résoudre des problèmes complexes. 
On raisonne de façon systémique. 
On ne se contente pas de lancer une idée, une théorie. 
On se doit de la structurer sous forme de proposition. 
Cette proposition s'appuie sur 3 à 5 raisons. 
Ces raisons doivent être bien compréhensibles, fiables (on justifie pourquoi), pertinentes (dans le contexte), convaincantes (claires, considérer les différents points de vue). 
On recherche aussi ce qui pourrait mettre à mal la proposition. 
On est transparent sur les points faibles identifiés. 
Il faut aussi préciser pourquoi cela est important, maintenant. 
J'avais abordé le sujet de la résolution de problèmes complexes en deux parties ([première]({{ site.url }}2024/02/13/Résoudre-des-problèmes-intuitivement.html) et [deuxième]({{ site.url }}2024/02/14/Résoudre-des-problèmes-rationnellement.html)). 
*Learning Systems Thinking* vient merveilleusement compléter ces lectures, et pousser la réflexion encore plus loin. 

## Modéliser
Une autre pratique fondamentale : la modélisation. 
Modéliser est une action qui nous permet de se faire une opinion. 
On génère un *artefact* (document visuel, écrit, code, etc) qui permet de partager la compréhension à un instant T du problème. 
On peut modéliser au cours d'un atelier comme un [Event Storming](https://www.eventstorming.com/). 
Mais ce n'est qu'un exemple. 
Tout type d'atelier, toute pratique collective peut nous aider si elle nous permet de créer un espace d'échange, de débats.  
On peut démarrer seul, mais il faut toujours à un moment ou un autre travailler en équipe. 
On expose notre pensée, on échange sur les idées. 
Travailler collectivement permet de partager ses perspectives. 
De ne pas être limité par ses angles morts. 
C'est [XP](https://en.wikipedia.org/wiki/Extreme_programming) qui met la communication comme valeur la plus importante. 
On retrouve cette importance aussi dans la pensée systémique. 
Pas d'intégrité conceptuelle sans collectif. 
Pour être sûr de se comprendre, il est utile de s'aider d'outils visuels, physiques ou virtuels.  
Et non, modéliser ce n'est pas faire du *big design up-front* comme cela est parfois objecté. 
C'est prendre le recul nécessaire, réfléchir, à plusieurs. 
Ce n'est pas faire une spécification qui trace la route pour les prochains mois. 
Ça, c'est le domaine de la pensée linéaire. 
Modéliser permet au système d'être en harmonie, plutôt que de se contenter d'une réponse locale incomplète. 
On regarde plus loin que ce qui est visible d'un coup d’œil, en prenant en compte les spécificités du contexte. 
La modélisation est une pratique indispensable pour nous guider vers des décisions avec plus d'impact, et plus justes. 

## Observer
Pour modéliser correctement il faut savoir observer. 
Regarder ce qui a mené aux événements à l'origine de la situation. 
Les motifs qui se répètent (ne serait-ce pas la troisième fois que nous avons ce type de bug ?). 
Pourquoi ces *patterns* se répètent ? 
Est-ce l'organisation qui motive cela ? 
Par exemple si l'organisation compare les vélocités des développeurs, cela peut amener à des comportements qui vont réduire la qualité du code, et donc créer plus de bugs. 
Et derrière tout cela, quels sont les modèles mentaux, croyances ou valeurs partagées dans l'organisation qui soutiennent la situation ? 
La valorisation de ceux qui produisent beaucoup, sans prendre en compte la qualité de ce qui est produit, ni la qualité de la participation à la dynamique d'équipe ? 
Est-ce que ceux qui prennent le temps d'aider les autres et qui donc produisent moins sont réprimandés ? 
L'échec est-il puni ?

Une image employée dans la pensée systémique est celle de l'iceberg. 
Elle nous permet de comprendre l'importance de l'observation. 
Le visible est constitué des événements. 
C'est ce que nous voyons tout de suite. 
Si on reste au niveau des événements, on reste dans la pensée linéaire. 
On ignore les relations. 
On sera dans la réaction et non dans la réponse. 
En observant nous pouvons alors accéder à ce qui est moins ou pas visible au premier abord. 
Il y a-t-il des tendances, des répétitions de schéma (n'est-ce pas la 3ème fois que ce type de bug survient) ? 
Qu'est-ce qui favorise l'apparition de ces schémas, les structures, les relations ? 
Quelles sont nos croyances, nos valeurs qui peuvent en être à l'origine ?
![Iceberg model]({{ site.url }}images/Iceberg Model.jpg)  
[Iceberg Model](https://www.flickr.com/photos/rosenfeldmedia/52597293290)  

## Deep Work
Et pour pouvoir faire ce travail de modélisation, d'observation, il faut avoir des temps de travail continus, sans distraction. 
Diana Montalion fait référence au livre *[Deep Work](https://calnewport.com/deep-work-rules-for-focused-success-in-a-distracted-world/)* de Cal Newport. 
Un livre qui nous invite à retrouver du temps de concentration et à éviter les multiples distractions. 
J'y reviendrai plus en détail dans un autre article, car la lecture de Deep Work mérite d'être partagée. 

## Respecter le travail des autres
Pour modéliser il faut aussi respecter le travail des autres. 
Cela me fait penser au Scout Mindset ([un article sur le livre de Julia Galef]( {{site.url }}2023/02/04/Fiche-de-lecture-The-Scout-Mindset-de-Julia-Galef.html)). 
On développe son esprit critique, on discute, on exprime ses accords et désaccords. 
C'est un travail personnel qui permet cela, et l'organisation peut aussi favoriser cet état d'esprit. 

## Améliorer la connaissance de soi
Pour développer ces compétences, il faut améliorer la connaissance de soi (*mindfulness*). 
C'est une pratique quotidienne qui est indispensable pour progresser en pensée systémique. 
Elle propose pour cela de s'appuyer sur des rituels d'écriture. 
Par exemple, prendre le temps d'écrire tous les matins. 
Sans forcément avec un but précis, au départ. 
Mais d'autres pratiques qui nous permettent une certaine introspection sont intéressantes. 
La méditation, la marche par exemple. 
A chacun de trouver son moyen. 
Ainsi, il nous sera plus facile de garder notre calme dans des situations tendues. 
De prendre le temps plutôt que d'être dans la réaction. 
De prendre conscience des [raisonnements fallacieux](https://yourlogicalfallacyis.com/fr) et des [bais cognitifs](https://fr.wikipedia.org/wiki/Biais_cognitif) à l’œuvre. 

Toutes ces pratiques nous permettront de nous rapprocher de l'intégrité conceptuelle. 
Elles sont toutes des chemins, sans fin. 

# Pratiquez-vous la pensée systémique ?
Pour savoir si vous êtes sur ce chemin, je vous partage ici une check-list que donne l'autrice pour savoir si on "pense système" :
* on pense système, seul et en équipe, quotidiennement. 
  C’est une compétence comme une autre.
* on sait décrire la pensée linéaire et non linéaire, et on sait quand utiliser l’une ou l’autre ou les deux.
* les personnes et la technique sont inextricables. 
  Les systèmes informatiques sont sociaux techniques.
* on a notre compréhension de l’intégrité conceptuelle
* on sait que les systèmes peuvent être contre-intuitifs, et donc on regarde dans quelle mesure on pourrait empirer les choses
* quand on fait face à des problèmes récurrents on ne blâme pas, pas de cargo culte (on évite les raisonnements fallacieux). 
  On cherche les points de bascule, plutôt que de mettre un pansement.
* on regarde le tout et les relations. 
On regarde sous les événements, les patterns, les structures, les modèles mentaux.
* on accepte que l’incertitude soit présente.
* on écoute, demande les autres perspectives
* on est capable d’expliquer son raisonnement, ce que l’on pense ne pas savoir
* face à des situations difficiles on fait preuve de connaissance de soi
* on apprend tout le temps
* le raisonnement systémique est le mode de communication par défaut
* on prend toujours en compte le contexte
* quand on déploie du code, on comprend ce qu’il apporte au système
* on utilise divers techniques de modélisation
* on encourage le flux de la connaissance

# Conclusion
Si certains points de ce que je viens de lister vous paraissent flous, ou si vous pensez devoir approfondir certains éléments, alors le livre vous apportera une partie de la lumière nécessaire. 
Et le livre est bien plus riche que ce petit tour d'horizon sélectif que je viens de vous faire. 
Il propose des exemples concrets qui permettent de bien comprendre le propos. 
On peut mettre en pratique très vite ce que nous lisons. 
C'est un livre qui demande un certain investissement en temps et en énergie. 
Mais tant vous sera rendu en retour que je ne peux que vous le recommander. 

Merci à Caroline, Florine et Fabien pour leurs retours précieux. 