# Comment j'écris des tests unitaires

Les tests sont un des sujets qui m'occupe particulièrement cette dernière année.
Et si c'est un sujet vaste où j'ai encore beaucoup à apprendre, je commence à me sentir enfin plus à l'aise sur les tests unitaires. 
Donc je vais vous partager quelques pratiques que j'essaie de suivre quand j'en écris. 
Elles sont pour la plupart issues de lectures (voir en fin d'article), associées à de la pratique régulière sur les projets pro et des katas. 
Je précise que ces pratiques sont des lignes directrices, et comme beaucoup de pratiques de développement, il faut faut savoir s'en écarter quand cela s'avère utile. 

## Why

Tout d'abord, voyons pourquoi j'écris des tests unitaires.

### C'est un outil de conception 

Pour moi aujourd'hui le plus important est leur capacité à faire émerger le design. 
En pratiquant TDD la plupart du temps je conçois grâce aux tests. 
Les tests étant un client comme un autre des abstractions  (interfaces / contrats / API), leur émergence correspond à un vrai besoin. 
La qualité des abstractions étant un élément fondamental dans la complexité des logiciels, les tests contribuent ainsi directement à une meilleure qualité. 
Je ne ferai pas ici de description  de TDD car je n'ai pas encore assez de bouteille sur la pratique pour me permettre d'en parler ici. 
Je peux simplement vous partager que je ne code plus mes tests autrement, et si parfois il m'arrive de bloquer et de revenir au papier crayon et aux diagrammes de séquence, je finis toujours par revenir au TDD. 

Si le sujet vous intéresse, je vous invite à suivre un de ses plus fervent défenseur en France, [Michaël Azerhad](https://www.linkedin.com/in/micha%C3%ABl-azerhad-9058a044/?originalSubdomain=fr). 

### C'est de la documentation

Avec des tests de qualité, il est plutôt facile de comprendre le fonctionnement d'un logiciel. 
Nul besoin de regarder le code et de lire des implémentations. 
Le code des tests nous permet de comprendre le besoin. 
Par exemple, quand je fais une revue de code , je commence par la lecture des tests. 
Si ceux-ci sont bien écrit je comprends plus vite le besoin et la conception que si j'attaquais directement par le code de production. 

### C'est un filet de sécurité

Pour détecter un bug mais aussi et surtout lors d'un refactoring. 
Les tests sont verts ? 
On *refactor* en toute sécurité et ce n'est pas rien. 

### C'est un révélateur de *code smell*

Un test difficile à écrire est un révélateur d'un problème de conception. 
J'essaie de profiter autant que possible de ce signal pour remttre en question la  qualité  de ce que je suis en train d'écrire. 
Si mon test devient complexe, ce sera probablement le cas pour mon code de production, que ce soit pour les clients de mon API ou de l'implémentation. 

## Quand écrire un test unitaire ?

La raison d’être d’un test est un nouveau comportement. 
C'est la raison principale, et c'est le déclencheur le plus fréquent. 

Parfois ça peut être des tests plus exploratoires, par exemple pour découvrir une API tierce, ou du code non couvert. 

## Quelles sont les frontières d'un test unitaire ?

Bon, je ne vous cache pas que c'est encore souvent un point dur sur lequel il m'arrive encore de buter. 
Dans les katas moins, mais dans un projet pro avec beaucoup de complexité j'au plus de mal à trouver la bonne frontière. 
Mais les lignes directrices suivantes m'aident. 

Trop souvent les tests vérifient des détails d'implémentation. 
Et très souvent je vois des *stubs* qui sont vérifiés alors que c'est un anti pattern comme indiqué plus haut. 
Les tests sont un client comme un autre de notre API, ils doivent donc connaître uniquement l'abstraction qu'ils testent. 

Quand un ensemble de classes collaborent étroitement ensemble, je teste les fonctionnalités offertes par la classe qui expose cela via son API. 
Par exemple si un ensemble de classes sont impliquées dans un calcul complexe, je ne teste pas indépendamment chaque classe, mais l'ensemble des classes.  
Ce qui va définir quels collaborateurs j'embarque et le périmètre de mon test : 
* le lien entre les collaborateurs (et qui contrôle la relation)
* la complexité combinatoire : plus il y a de collaborateurs, plus il y a de chemins. Il y a donc des fois où il faudra soit ignorer des chemins (on prend le système comme une "boite grise" et non comme une "boite noire"), soit introduire des *tests doubles* pour limiter la complexité. 
* la performance, mes tests doivent être rapides. Par exemple je n'embarque pas les DAOs, je les *mock*, même si certains recommandent l'usage de [testcontainers](https://www.testcontainers.org/). 

## Une classe par fixture

Il est possible que la nécessité d'écrire beaucoup de tests soit un 
*code smell*. 
C'est peut être un signe que notre SUT a trop de responsabilité. 
Mais ce n'est pas toujours le cas. 

Quoiqu'il en soit, une classe de test trop grande devient vite illisible, et chaque développeur qui arrive sur ces classes aur tendance hélas à rajouter son cas de test plutôt que de faire l'effort de refactorer. 
On finit par se retrouver avec des tests dont la couverture se recoupe et qui deviennent inmaintenables. 

C'est pourquoi j'aime regrouper mes tests quand c'est nécessaire, soient  dans des fichiers séparés, soit en utilisant les *inner class* avec `@Nested` de [JUnit](https://junit.org/junit5/docs/5.4.1/api/org/junit/jupiter/api/Nested.html). 
C'est la *fixture*, c'est à dire ce qui permet d'initialiser les tests, qui me guide pour le regroupement de tests. 
Tous les tests qui partagent la même *fixture* sont regroupés. 
  
## Nommage des tests

### Nommage de la classe

Certains nomment leurs classes avec le suffixe `Should`. 
En général je me contente de mettre `[SystemUnderTest]Test`. 
Si j'ai plusieurs classes pour mon SUT, alors je précise ce qui regroupe les tests. 

### Nommage des méthodes

Une méthode de test se lit comme une phrase. 
La phrase commence implicitement par l'objet du test, et la méthode de test indique ce qui est fait. 
Par exemple pour un objet `Travel`, qui a une méthode `ItineraryType itineraryType()` pour déterminer le type du voyage (international ou domestique), j'ai une méthode de test `isDomesticWhenAllStationsAreInFrance`. 
On lit donc la phrase "*Travel is domestic when all stations are in France*". 
Vous pouvez voir d'autres exemples de ce nommage sur le [wiki de Ward Cunningham](http://wiki.c2.com/?SentenceStyleForNamingUnitTests). 

Au boulot on applique une convention assez proche : chaque test commence par `should`, en séparant les mot par des *underscores*, ce qui donnerait pour l'exemple précédent : `should_be_domestic_when_all_stations_are_in_france`. 

Le nommage du test ne sera peut-être pas correct du premier coup, mais il est important qu'à la fin il le soit. 
C'est une question à se poser à la phase de refactoring en TDD. 

### Nommages dans les tests

Pour rapidement identifier ce qui ne marche pas dans un test ko, il est intéressant d'affecter des valeurs descriptives aux variables par exemple "id du client valide", plutôt que "abcde12345". 
Cela peut nous encourager à définir nos propres Value Types pour surcharger la méthode `toString`.  
Il faut donc jouer sur le nom de la variable **et** sur son contenu.

Aussi, au lieu de passer `null` en paramètre, cela peut valoir le coup de nommer une variable qui est elle `null`. 
Cela facilite la lisibilité (oui, `null` en paramètre c'est moche). 


## Structure

Concernant la structure, deux nommages sont pratiqués et équivalentes. Il suffit d'en choisir un et de s'y tenir.  

```
// given ou arrange
ici le setup du test
// when ou act
l'appel au system under test
// then ou assert
les assertions
```

### Assertions

Après avoir nommé le test, j'enchaîne par les assertions, en général. 
Il peut m'arriver plus rarement de commencer par le *when*.
En tout cas, jamais par le *given*. 

Tant que les assertions testent une feature cohérente, on peut en avoir autant que nécessaire. 
Il est en revanche inutile de répéter des assertions déjà vérifiées dans un autre test et c'est hélas une pratique que je trouve fréquemment et qui alourdit les tests inutilement. 

Que doivent vérifier nos assertions ? 
J'applique ce que Sandi Metz [a très bien résumé](https://www.youtube.com/watch?v=URSWYvyc42M) dans sa matrice.  
On a des messages qui soit rentrent dans le SUT, soit sont internes au SUT, soit en sortent. 
Ces messages sont soit des *queries* (on attend un résutat) soit des *commands* (on attend un effet de bord). 
En fonction de ces types on effectue, ou pas, différents types de vérifications. 

| Message | Query | Command |
| ------- | ----- | ------- |
| Entrant | Assertions sur la réponse du SUT (sur l'état) | Assertions sur les effets de bord directs et publiques |
| Interne | Ne pas tester | Ne pas tester |
| Sortant | Ne pas tester | Tester uniquement ce qui est de la responsabilité du SUT. Vérifier avec un mock si le message a bien été envoyé |


On voit donc qu'à aucun moment on ne fait d'assertion sur un collaborateur qui retourne un résultat (sortant / *query*), hors c'est une pratique que je vois très souvent, sous forme de *verify*  sur des *stubs* (avec Mockito par exemple en Java). 

Pour aller plus loin : [When to mock](https://enterprisecraftsmanship.com/posts/when-to-mock/) et [Mocks aren't stubs](https://martinfowler.com/articles/mocksArentStubs.html). 

### When

Ensuite vient l'écriture du *when*, l'appel à notre SUT (System Under Test). 
Normalement on sait déjà ce que l'on attend comme retour car on a déjà écrit les assertions.  
On doit donc finir de décrire  l'abstraction de notre objet testé. 
Cette phase est très importante, la qualité des contrats (des abstractions plus globalement) étant fondamentale pour réduire la complexité des applications. 

### Given / Arrange

Puis on finit par la déclaration de ce qui est nécessaire pour initialiser le test. 

## Le chemin est encore long

Nous voilà à la fin de ce petit panorama de pratiques que j'applique pour mes tests. 
A travers cet article c'était une façon pour moi de [tester mes connaissances sur les tests unitaires](https://en.wikipedia.org/wiki/Inception). 
Et si je n'ai fait que gratter la surface d'un sujet énorme, j'espère que vous y aurez trouvé quelques grains à moudre. 

****
Ressources : 
* [Test Driven Development By Example](https://www.oreilly.com/library/view/test-driven-development/0321146530/)
* [Growing Object-Oriented Software Guided by Tests](http://www.growing-object-oriented-software.com/)
* [Practical Object-Oriented Design](https://www.poodr.com/)
* [Don't be mocked by your Mocks: Listening to your Tests](https://www.youtube.com/watch?v=pKBjufM024U&list=PLggcOULvfLL_MfFS_O0MKQ5W_6oWWbIw5&index=20)
* [Ian Cooper, conférence "TDD, Where Did It All Go Wrong"](https://www.youtube.com/watch?v=EZ05e7EMOLM)