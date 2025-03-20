Résoudre des problèmes intuitivement n’est pas toujours une bonne idée. Mais pour certains problèmes ce n’est même pas envisageable. 
La complexité nous invite automatiquement à raisonner. Mais sommes nous équipés, outillés, formés à la résolution de tels problèmes ?

Il m’est arrivé trop souvent de ne pas avoir la méthode et la rigueur pour résoudre de tels problèmes, et j’ai fréquemment observé la même chose chez des collègues également. Les conséquences sont plus ou moins désastreuses. 
Du code qui devient très compliqué à maintenir. Un nouveau besoin qui est mal analysé, découpé, et dont la réalisation va s’étaler sur beaucoup plus de temps que nécessaire, et générer beaucoup de stress.  
Peut-être possédez-vous déjà cette rigueur, personnellement elle m’a longtemps manqué.
Un cadre rassurant, la connaissance d’un fil à dérouler m’aurait aidé dans des situations passées où j’ai été débordé par la complexité. 
Puisse cet article aider un peu celles et ceux qui se lancent à corps perdu face à une montagne, en prenant un chemin hasardeux, parfois douloureux.

Avec une bonne organisation et méthode, on peut se simplifier grandement la tâche.  
Dans le livre *Bulletproof problem solving* les auteurs proposent une succession de 7 étapes. 
Ce livre étant avant tout destiné à des consultants traitant des problèmes très complexes et au long cours, j’ai mis de côté certains conseils. 
Je pense par exemple que les techniques d’analyses basées sur du machine learning, ou basées sur de la théorie des jeux, etc. ne me seront jamais utiles. 
Je vous présente donc une partie allégée de ces étapes, qui, il me semble, sont intéressantes à appliquer dans les différents types de problèmes que l’on rencontre régulièrement dans la vie d’un développeur. 

![7 étapes]({{ site.url }}images/bulletproof.png)

*Les 7 étapes, image issue du livre Bulletproof Problem Solving*

En préambule je précise tout de suite que ces étapes ne forment pas un processus rigide. 
On peut itérer, revenir en arrière si le feedback de nos avancées nous fait porter un nouveau regard sur nos premiers pas. Agile quoi. 

La première étape donc, c’est de définir le problème. 
Passer d’une vague définition à une définition précise. 
On veut éviter de résoudre un autre problème et de passer du temps sur des actions qui seraient inutiles. 
Pour ce faire, tout ce qui va nous aider à lever les ambiguïtés sera noté. 
Plus le problème sera complexe, plus le travail de définition sera long. On indiquera les frontières du problèmes, le contexte, ce qui détermine le succès de sa résolution, si besoin les parties prenantes, etc. 
Dans certains cas on pourra s’inspirer des [objectifs “SMART”](https://fr.wikipedia.org/wiki/Objectifs_et_indicateurs_SMART). 
Dans cette phase, comme tout au long de la résolution, on ne se contente pas de ce qui nous est servi sur un plateau. 
On [demande pourquoi en boucle](https://en.wikipedia.org/wiki/Five_whys) (on le fera tout au long de la résolution). 
Ce point de départ est important pour ne pas s’éparpiller et résoudre plusieurs problèmes en même temps. 
Y passer un long moment ne doit pas décourager : c'est un signe indirect de l'importance de ce travail préparatoire (puisque ce temps est corrélé positivement à la complexité du problème à traiter.)
Cela nous évitera aussi de tomber dans le piège de la solution facile, intuitive, comme présenté [dans le premier article]({{ site.url }}2024/02/13/Résoudre-des-problèmes-intuitivement.html). 
Voici deux exemples de problèmes : 

Exemple 1. Une équipe Data qui fait des reporting posent des questions sur les valeurs “étonnantes” dans des champs contenant l’adresse de clients. 
- Définition du problème : “Tracer le chemin depuis l’input jusqu’à l’écriture en base des champs afin de déterminer tout ce qui influence la donnée qui sera écrite”.
- Valeur : Permet de débloquer le reporting de la data. La demande sera considérée résolue quand tous les chemins seront analysés. On doit donc s’assurer d’avoir les moyens de trouver de façon exhaustive tous les chemins.
- Mesure du succès : on a un document qui détaille les chemins vers la données de façon claire.
- Temps : Non contraint.
- Contraintes particulières : Il va falloir analyser sur tout le périmètre, car tout le monde peut écrire directement en base de données.

Exemple 2. Une demande réglementaire d’ajout d’une fonctionnalité d’identification d’incohérences de données client. 
Cela impacte l’équipe qui gère la souscription, celle qui gère l’espace client, une autre qui gère l’application mobile, etc. Ce problème est bien plus complexe que sur le précédent.
Il sera utile de l'imaginer comme une composition de nombreux sous problèmes. 
- Définition du problème : Vérifier les incohérences des données clients, que ce soit lors de la création d’un compte ou lors d’un changement en cours de vie.
- Valeur : Mise en conformité avec la loi
- Mesure du succès : Les incohérences peuvent être détectées et leur cycle de vie géré.
- Temps : Doit être prêt au 31/12/2023
- Contraintes : Pas d’équipe de dev ayant la responsabilité de ce périmètre. Impacts sur plusieurs équipes.
- Acteurs : équipes réglementaires, les équipes de devs, le pôle réglementaire.

Dans une seconde étape on va déconstruire le problème pour voir les chemins menant à sa résolution, éviter ceux qui ne mènent nulle part, ou qui sont incertains. 
Cette étape est très importante, et il me semble que je ne la vois pas souvent appliquée autour de moi. 
Cette déconstruction se partage visuellement, elle sert de support de communication. Elle est représentée par des arbres. 
Peu importe le type. 
La seule caractéristique commune de tous les types d’arbre : les branches sont mutuellement exclusives, elles ne se chevauchent pas dans leur périmètre, et sont collectivement exhaustives, le tout représente l’ensemble du problème. 
L’arbre peut être inductif, en partant des hypothèses et en allant vers la solution, ce qui peut être utile si on a déjà une vision sur des terminaisons de l’arbre, mais sans maitriser toutes ses relations. 
Il peut être plus classiquement déductif, en partant du problème. 
Chaque branche étant un chemin à explorer.  
L’arbre va être notre carte pendant notre exploration, il nous évitera de nous perdre. 
Et nous permettra de prendre de la hauteur face à la complexité, de communiquer sur nos avancées. 
Cette pratique m’aide beaucoup pour calmer mon stress face à la complexité. 
Le problème devient déjà moins complexe, et plus palpable. 
En terme d’outil j’utilise [draw.io](http://draw.io) ou une mindmap avec xmind, mais il y en a bien d’autres qui pourraient être utilisés. 
Il faut qu’il permette facilement de créer les arbres, d’ajouter des notes, et de visualiser rapidement ce qui est en cours et ce qui a été traité (j’utilise des codes couleurs). 
Cette seconde étape a été un changement très positif dans mon organisation, et l’apport le plus significatif de la lecture du livre pour moi.  

*Note : La [méthode mikado](https://mikadomethod.info/) propose aussi de faire des arbres. C’est une méthode qui permet d’adresser la complexité d’un refactoring complexe. On complète un graphe au fur et à mesure de nos avancées, avec la particularité de faire un rollback dès que l’on tombe sur un os. Je ne rentre pas dans les détails de cette méthode, mais je vous recommande vivement de l’ajouter à votre trousse à outils si vous ne la connaissez pas (par exemple en lisant [cet article](https://improveandrepeat.com/2020/12/the-mikado-method-a-great-help-to-work-with-legacy-code/) ou [celui-ci](https://arnolanglade.github.io/mikado-method.html)). Ce point commun qui fait de l’usage d’arbres pour résoudre une problématique complexe souligne il me semble l’importance de ce support, à systématiser dans ces situations.*  


![Karen Arnold]({{ site.url }}images/arbre.png)

*[Karen Arnold](https://www.publicdomainpictures.net/fr/view-image.php?image=248573&picture=peinture-d39arbre-vintage)*

Dans une troisième étape, muni de cette carte on pourra décider si tous les chemins doivent être empruntés ou pas. 
Dans certains cas, on devra faire un parcours exhaustif. 
Dans d’autres on pourra élaguer, et prioriser les chemins les plus importants. 
Si nécessaire, une matrice de décision 2×2 (celle d’[Eisenhower](https://fr.wikipedia.org/wiki/Matrice_d%27Eisenhower) en est une déclinaison) peut nous aider à faire cette priorisation. 
Mais en général, pour les problèmes que je rencontre, je peux m’en passer. 

En 4ème étape, là aussi pour les problèmes qui en ont besoin, on planifie. 
Soit pour faire des analyses, soit pour commencer des travaux de résolution. 
Par exemple dans le cadre du désendettement d’un legacy, on aura beaucoup de chantiers identifiés aux étapes précédentes. On devra parfois mener des analyses avant d’en démarrer, parfois non. 
On ne planifie pas non plus trop loin : ce serait inutile car l’inconnu changera probablement l’avenir. 

5ème étape : c’est parti pour les analyses et le premiers travaux. 
On itère, on améliore. 
On précise éventuellement notre définition du problème, et on complète notre arbre/carte. 
Ces analyses peuvent elles-mêmes être abordées comme des plus petits problèmes à résoudre, et bénéficier des 7 étapes.

6ème et 7ème étapes. 
Si on doit partager notre résultat, on prendra soin de bien communiquer et d’exposer notre travail dans une synthèse. 
Un beau support ne sera pas à négliger. 
Tous ces efforts pour ne pas être compris, ce serait dommage, et pas très respectueux non plus. 
Et l’aspect documentaire est très important pour l’avenir. 
D’autres personnes seront peut-être amenées à travailler sur la même problématique que vous. 
Mettez donc à disposition votre analyse en annexe. 

Ces étapes vous paraissent évidentes ? Super ! 
Vous avez déjà certainement une rigueur qui vous permet de résoudre des problèmes complexes. 
Si en revanche vous vous sentez parfois débordés, démunis au pied d’une montagne, essayez les. 
Dessinez votre carte. 
Cela ne résoudra pas le problème à votre place, certes. 
Mais la charge mentale des parties prenantes sera allégée, la communication plus simple, l’organisation et le chemin vers la résolution moins chaotiques et moins angoissants.
