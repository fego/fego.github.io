Tester des scripts SQL n'est pas simple dès que le script comment à avoir une certaine complexité. 
Il faut avoir un environnement disponible qui représente les conditions de production. 
On ne doit pas abîmer les données des collègues. 
On doit pouvoir ré-exécuter les tests de nombreuses fois. 
Réaliser cela directement sur un environnement de développement n'est pas toujours compatible avec ces conditions. 
Par exemple, comment remettre facilement un jeu de données en état quand on appelle une procédure qui clôture le compte d'un client, sans que l'on ait un moyen simple de rouvrir le compte ? 

![Complexité]({{ site.url }}images/complexity.jpg)  
*[Photo](https://unsplash.com/fr/photos/YTKh06aL7to) de Michal Matlon*

Dans ma mission actuelle cette situation m'est arrivée. 
Je me suis retrouvé à développer un script avec un curseur qui appelait 3 procédures stockées, dont 2 qui avaient des effets très difficiles à annuler (une clôture de compte et une opposition de carte). 
Cette difficulté m'a poussé à limiter mes tests (consciemment et inconsciemment). 
J'ai testé chaque cas séparément mais l'ensemble du script a bénéficié de beaucoup moins de tests. 
Et j'ai laissé passer une erreur. 
Cette erreur est passée aussi en revue, et en test manuel pour les mêmes raisons.
Pour finir par être exécutée en production. 
Heureusement les conséquences n'étaient pas grave, juste un coup de chaud. 
Et l'impression désagréable de passer pour un clown. 

Ce goût amer restant dans ma bouche, et surtout l'insatisfaction des conditions de développement de ces scripts m'ont poussé à creuser ce que je pouvais améliorer pour me donner plus de confiance dans l'écriture de ces scripts. 

C'est ainsi que j'ai mis en place le mini projet [test_sql_starter](https://gitlab.com/davfranck/test_sql_starter). 
Ce projet permet de mettre rapidement en place un environnement pour tester son script dans des conditions de production, ou presque. 
En effet, en s'appuyant sur la lib testcontainers, on peut exécuter notre script sur le SGBD de notre choix. 
Dans mon cas le test de mon script fait les actions suivantes : 
* initialisation de la base, du schéma, des tables à l'identique de la production. 
* je crée aussi une table de logs spécifique à mon test
* j'initialise les procédures stockées. 
Je modifie les procédures afin de simplifier l'exécution du script. 
Par exemple si ma procédure a un effet de bord comme simplement appeler une autre procédure, alors j'insère dans la table de log une ligne prouvant que l'appel a été effectué. 
Ou si ma procédure retourne une donnée, je retourne une valeur en dur en fonction du paramètre. 
Bref, comme j'utiliserais un [test double](https://en.wikipedia.org/wiki/Test_double) pour un test unitaire classique. 
* j'initialise ensuite les données dont j'ai besoin
* puis j'exécute mon script
* enfin je vérifie si tout c'est bien passé : 
  * à la fois en interrogeant les tables métiers 
  * mais aussi en vérifiant ma table de logs

Je me retrouve donc avec un environnement rapide, fiable, qui en plus me permet d'avancer en baby steps si je le souhaite et donc de faire du TDD. 
Toutes les instructions sont compatibles avec l'environnement cible, car exécutée de la même façon. 
Il suffira de prendre le script et de l'exécuter dans un environnement cible. 
Rien à retoucher. 
Bien entendu en passant au préalable par un environnement de développement, puis de recette. 

Rien de bien compliqué, mais ce setup permet de gagner du temps pour la mise en place de ces bonnes conditions. 
J'ai déjà pu tester ce projet sur un nouveau script et le confort qu'il procure me convainc que je ne développerais plus de scripts sans lui. 

C'est par là : [test_sql_starter](https://gitlab.com/davfranck/test_sql_starter). 
Le projet est bien entendu ouvert à toute amélioration (PR), comme par exemple l'ajout d'un nouveau wrapper pour une autre base de données. 