# Accelerate

Je vous partage ici quelques notes suite à la lecture du livre [_Accelerate_](https://itrevolution.com/book/accelerate/) de Nicole Forsgren, Jez Humble et Gene Kim. 

Depuis quelques années les auteurs réalisent des rapports nommés "state of devops", ce livre en révèle les découvertes et la méthode. Les auteurs montrent comment les pratiques "devops" permettent d'augmenter la performance des entreprises et leurs revenus, que c'est une clé de leur réussite.  Mais qu'est ce que "[devops](https://en.wikipedia.org/wiki/DevOps)" ? C'est à la fois des pratiques techniques, des process lean, et une culture. C'est ces différents aspects qui seront regardés à la loupe. 

Le but de ma lecture étant surtout de découvrir et comprendre les clés de la performance, je fais l'impasse sur la partie méthodologie, chapitres cependant essentiels pour donner du poids aux conclusions du livre. Celle-ci étant celle d'un développeur, je suis certainement passé à côté de pas mal d'idées, et j'en ai écarté aussi certaines volontairement. J'espère vous donner envie de lire le livre.

## Mesure de la performance

Dans le livre les auteurs nous montrent ce que font les meilleurs performeurs par rapport aux autres, afin de pouvoir dégager ce qui les rends meilleurs. Comment font-ils pour à la fois être rapides et stables, sans contrepartie ?
On commence par s'intéresser aux mesures de la performance.

Les auteurs critiquent les modèles de maturité, (comme [CMMI](https://en.wikipedia.org/wiki/Capability_Maturity_Model_Integration)) car 

* ils sont statiques, on a tendance à se figer quand on arrive au niveau le plus élevé, alors qu'il faudrait continuer sans cesse de se transformer. 
* ils ne prennent pas en compte les différences entre les organisations

Il faut donc utiliser des modèles basés sur les capacités. Une bonne mesure doit avoir deux caractéristiques clés : 

- être focus sur un revenu global pour assurer que les équipes ne sont pas en concurrences les unes avec les autres.
- être focus sur les revenus et non sur ce qui est produit (et qui ne génère pas forcément des revenus)

Cela a abouti à 4 mesures : 

- délai de mise en production à parti d'un commit. (_Catégories : moins d'1 h, moins d'1 j, entre 1 j et 1 semaine, entre 1 semaine et 1 mois, entre 1 mois et 6 mois._)
- fréquence de déploiement, et son corollaire la réduction de la taille des livraisons. (_Catégories : à la demande, entre 1 par heure et 1 par jour, entre 1 par jour et 1 par semaine, entre 1 par semaine et 1 par mois, entre 1 par mois et 1 tous les 6 mois, moins de 1 fois tous les 6 mois._)
- temps pour restaurer un service
- taux d'erreurs sur les changements

En 2019 dans le rapport figure une nouvelle catégorie, dite "élite", voici ce que font ces meilleurs performeurs par rapport aux moins bons[^3] (dans le livre vous trouverez les chiffres de 2017 qui sont déjà assez impressionnants) : 

![b468307df3f84d12d1079cde08d1ed5e.png]({{ site.url }}images/b468307df3f84d12d1079cde08d1ed5e.png)

## Développer la culture d'une organisation

Maintenant que vous savez comment vous vous situez en niveau de performance, voyons ce que propose le livre pour vous améliorer.

On commence par l'aspect organisationnel. Le constat : les organisations avec un bon flux de communication fonctionnent de façon plus efficace. 

Ils distinguent 3 typologies des organisations, basées sur le travail de [Ron Westrum](https://en.wikipedia.org/wiki/Ron_Westrum) : 

- pathologique, orientée pourvoir
- bureaucratique, orientée règle
- génératrices, focus sur la mission.

Une bonne culture requiert de la confiance et de la coopération. Elle permet des bonnes prises de décision et la possibilité de revenir en arrière car plus ouverte et transparente.

Ils citent une étude de Google qui a conclu en 2015 : "qui" est dans l'équipe importe moins que comment les membres de l'équipe interagissent, structurent leur travail et voient leurs contributions. Bref, **tout vient de la dynamique d'équipe**.  Cela me rappelle [ce podcast avec Arnaud Lemaire](https://compagnon.artisandeveloppeur.fr/feed-entries/podcast/le-facteur-de-performance-1-d-une-equipe-avec-arnaud-lemaire) que je vous conseille. 

Comment changer la culture ? Non pas en changeant ce que les personnes pensent, mais ce qu'elles font.

J'ai eu un peu de mal à extraire la sève de ce chapitre malgré ces quelques éléments intéressants.

## Impact des pratiques techniques sur la performance

Beaucoup de méthodes agiles, comme Scrum, ont traitées les pratiques techniques (comme TDD, l'intégration continue, etc) comme secondaires. A la différence d'XP. Pourtant les pratiques techniques sont vitales pour la réussite.

### Continuous delivery

Une de ces pratiques est le continuous delivery (CD), soit la capacité de mettre en production de façon sécurisée, rapidement et durablement des features, de la config, des bug fixes, des expérimentations.

Travailler en continous delivery c'est :

- Permettre de détecter rapidement s'il y a des problèmes.
- Travailler en petit lot. Les organisations ont tendance à planifier le travail en gros morceaux. En splittant le travail en plus petits morceaux qui permettent de mesurer les revenus rapidement, nous avons le feedback essentiel sur le travail fourni, cela rajoute quelques frais, mais les bénéfices sont énormes pour éviter de délivrer des choses qui n'amènent aucune valeur.
- Laisser les tâches répétitives aux ordinateurs, les personnes résolvent des problèmes et peuvent travailler sur des problèmes à haute valeur ajoutée.
- Poursuivre sans relâche l'amélioration continue.
- Responsabiliser et impliquer tout le monde dans le delivery.

Pour implémenter le CD, il faut les fondations suivantes : 

- Tout automatiser à partir de ce qui se trouve dans le VCS (code, mais aussi la configuration).
- Limiter la durée de vie des branches (moins d'un jour) et les intégrer dans master régulièrement. Quasiment jamais de "stop commit" (code lock). Valable quelle que soit la taille de l'équipe, du projet, de l'orga.
- Jouer tous les tests (TU et acceptance) à chaque commit. Tous les tests doivent être fiables, ne pas produire de faux positifs. Si vous avez des tests instables, mettez les dans une autre "test suite" (de quarantaine). On doit pouvoir avant jouer tous les tests sur son poste en local et les testeurs devraient effectuer des tests exploratoires sur les derniers builds.

Que cela va-t-il apporter ? Moins de douleur lors du déploiement et moins de risque de burn out, une mise en prod n'est plus un big bang qui stress tout le monde. Cela influence aussi positivement la culture de l'organisation, le taux d'erreurs sur les changements. On peut alors se concentrer sur les nouveaux besoins, plutôt que de faire du rework. 

### Architecture

Hormis le cas ou le logiciel est outsourcé ou le cas d'un mainframe (et non l'intégration *avec* un mainframe), les types de systèmes n'influencent pas la performance. Cependant deux caractéristiques d'architecture sont importantes :

- capacité à tester la majorité de l'application hors environnement d'intégration
- capacité à deploy / release l'application indépendamment des autres applications / services dont on dépend.

Il faut donc des systèmes peu couplés. 

Pour une bonne performance il faut pouvoir : 

- faire des gros changements de design sans la permission de quelqu'un en dehors de l'équipe.
- faire des grands changements dans le design sans que cela implique beaucoup de travail pour les autres équipes
- faire son travail sans communiquer et se coordonner avec des personnes en dehors de l'équipe
- deploy et release à la demande, indépendamment des autres services
- faire la majorité des tests à la demande, sans nécessité d'avoir un environnement d'intégration.
- réaliser des déploiements pendant les heures normales de travail.

Et comme bien souvent quand on parle architecture, on parle aussi équipe tant les deux semblent s'influencer. 
Les équipes doivent être multi fonctionnelles (dev, ops, fonc, testeur). La communication inter équipe est très importante, mais on essaie de réduire le bruit pour laisser la place à l'essentiel[^2]. Il est bien entendu question de la fameuse [loi de Conway](https://fr.wikipedia.org/wiki/Loi_de_Conway), mais pour amener un concept qui vaut le coup d'être creusé : "[Inverse Conway Maneuver](https://www.thoughtworks.com/radar/techniques/inverse-conway-maneuver)", ou l'idée est d'adapter l'organisation à l'architecture cible. Bon, [sur le radar de thoughtworks](https://www.thoughtworks.com/radar/faq), ça a disparu assez rapidement...

Ils conseillent de permettre aux équipes de choisir leurs propres outils. Cependant il y a certains endroits ou des standards doivent s'appliquer (par exemple au niveau des plateformes). Les outils choisis doivent être appréciés par les développeurs, et répondre au besoin.

## Management

Quid du management sur l'impact de la perforamnce ?

### Lean management

Ce n'est pas un sujet que je connais, voici ce que j'en retient : le lean management va tendre à : 

- limiter le WIP
- créer des indicateurs visuels sur la qualité, la productivité, et le statut courant (et aussi failures, defect rate).
- utiliser les données de performance du logiciel et des outils de monitoring pour prendre des décisions business quotidiennement

Il impact positivement la performance et la culture de l'organisation.

### Leaders et managers

Etre un leader ce n'est pas faire des graphiques, c'est inspirer et motiver les personnes. Son rôle est essentiel pour : 

- établir et encourager une culture de confiance
- favoriser les techniques pour améliorer la productivité, réduire le délai de déploiement
- encourager les expérimentation et l'innovation
- travailler à travers les différents silos pour maintenir un alignement stratégique.

Les 5 caractéristiques d'un leader de la transformation : 

- a une compréhension claire de l'emplacement actuel de l'orga et de sa place dans 5 ans
- communique d'une façon qui inspire et motive, même dans un environnement incertain
- stimule intellectuellement, incite à penser différemment.
- montre de l'attention et de la considération au besoin et sentiments des autres
- loue et reconnaît les réussites et améliorations, complimente personnellement quand certains se surpasse.

Les équipes les plus performantes sont celles qui ont des leaders les meilleurs dans les dimensions ci-dessus, mais les leaders ne peuvent réussir les objectifs de performance seuls. 

La connaissance est le pouvoir, et vous devez donner le pouvoir à ceux qui ont la connaissance. 

## Retour d'expérience chez ING Pays Bas

Après un chapitre sur la méthodologie pour mener ces recherches, le livre s'achève sur un retour d'expérience de Steve Bell et Karen Whitley Bell chez ING Netherlands. Il est question d'organisation des équipes, avec un modèle que vous avez déjà [peut être rencontré](https://www.atlassian.com/agile/agile-at-scale/spotify) : 

![orga équipeschez ing](https://agilebusinessmanifesto.com/wp-content/uploads/2017/10/ING-agile-1.png)

Rapidement ça donne :

* tribu : ensemble d'équipes avec des interconnexions.
* squad : équipe de 9 personnes max, autonome, démantelée quand la mission est terminée. 
* chapitre : développe l'expertise et la connaissance de façon transverse.

Et pour clôturer en beauté ce livre, ils donnent leurs conseils, que je résumerai avec ces quelques phrases que j'ai bien appréciées : 

* Ne pas chercher à copier. Etudier et apprendre des autres entreprises, mais ensuite expérimenter et adapter à ce qui peut marcher pour vous et votre culture. Par exemple le modèle Spotify ne sera [peut-être pas adapté à votre organisation](https://www.jeremiahlee.com/posts/failed-squad-goals/)[^4] [^5]. 
* Ne pas demander à une grande société de consulting de transformer votre organisation. 
* Tout le monde doit changer sa façon de travailler, qui que ce soit. 
* Le changement demande de la discipline, du courage. 
* Pratiquez, pratiquez.

## Conclusion

Ce qui est dit dans ce livre va souvent dans le sens de l'intuition, enfin au moins de celle des personnes techniques il me semble, mais le gros point fort est d'amener si ce n'est des preuves, au moins des éléments forts de réponse à l'importance de la performance de l'IT.
Il pourra s'avérer être un bon compagnon lors de réflexions sur les organisations des équipes et de la mise en place des solutions techniques et accompagner le changement tout au long de la vie des projets.

Je remercie [Fabien](https://twitter.com/fhiegel) et Rémi pour leur relecture et bons conseils.

## Ressources

Quelques liens autour du livre si vous voulez vous faire un autre avis : 

* [Goodread](https://www.goodreads.com/book/show/35747076-accelerate)
* Une [revue courte](https://archiloque.net/blog/accelerate/) mais intéressante qui aborde quelques points que je n'ai pas remontés.
* [le rapport de 2019](https://services.google.com/fh/files/misc/state-of-devops-2019.pdf)
* Une [vidéo de 30mn](https://www.youtube.com/watch?v=UYiQMnkXLMQ) de Nicole Forsgren sur le rapport 2019

___

[^2]: sujet abordé par Adam Tornhill dans son livre [Software Design X-Ray](https://pragprog.com/titles/atevol/software-design-x-rays/)
[^3]: voir [le rapport 2019](https://services.google.com/fh/files/misc/state-of-devops-2019.pdf) p21 (et p18 pour la comparaison entre les différents niveaux)
[^4]: Citation issue de l'article : "_The co-author of the Spotify model and multiple agile coaches who worked at Spotify have been telling people to not copy it for years_"
[^5]: Autre citation de l'article : "_You might have discovered the Spotify model because you were trying to figure out how to structure your teams. Don’t stop here. Keep researching_"