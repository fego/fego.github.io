# Immutabilité et mutabilité, choisir en connaissance de cause

Pendant longtemps je n'ai vu sur mon chemin que des objets métiers mutables. 
C'était le cas bien souvent car nous n'avions qu'un même modèle du *controller* à la base de données. 
Sur mon projet actuel nous essayons de passer sur une architecture hexagonale. 
Il y a beaucoup de travail car beaucoup de code a été produit avant que nous ne prenions cette direction. 
Une des tâches est d'introduire un modèle métier, indépendant de celui de l'api et celui de la base. 
Et nous avons décidé de partir sur un modèle immutable. 
Enfin autant que possible. 
Ce que je vous partage ici n'est pas nouveau, mais pourrait vous intéresser si vous n'avez pas encore creusé ce sujet. 
J'espère vous en présenter une vue synthétique qui pourrait vous intéresser quand l'heure du choix s'offira à vous.

## Les bénéfices
En résumé : on a plus de lisibilité et moins de bugs. 
Voyons le détail.

### Identité de l'objet
Si on modifie l'identité d'un objet cela peut poser des problèmes dans les maps par exemple. 
Voici un exemple issu de l'article de [Yegor Bugayenko](https://www.yegor256.com/2014/06/09/objects-should-be-immutable.html). 

```java
Map<Date, String> map = new HashMap<>();
Date date = new Date();
map.put(date, "hello, world!");
date.setTime(12345L);
assert map.containsKey(date); // false
```
Et oui, le hash de la data n'est plus le même. 
Fort heureusement, la nouvelle api `java.time` est désormais immutable. 

### Atomicité de construction

Soit l’objet est créé, soit ça a planté, mais il ne peut être dans un état instable. 
Ses invariants sont validés à la construction. 
C'est un **énorme** avantage. 
On peut utiliser l'objet sans crainte et s'éviter des vérifications fastidieuses, de la programmation défensive. 

### Couplage temporel

Avec des objets immutables on ne peut séparer l’instanciation de l’initialisation, donc on évite ces problèmes. 

### Partageable sans crainte

Il n'y aura pas d'effets de bord. 
Avec l'immutabilité on n’aura jamais à investiguer où l’état a été modifié, et qui fait planter le programme avec un petit `setTitre(null)`. 
On peut les réutiliser partout ou nécessaire, comme par exemple `BigDecimal.ZERO`. 
A l'inverse avec un objet mutable on peut créer des bugs difficiles à résoudre. 
Par exemple un objet mutable issu d'un cache, si on le modifie on corrompt le cache. 

### Thread safety

Avec de l'immutabilité, on s'enlève de la complexité dans la gestion des accès concurrents et des bugs très difficiles à débugger.



## Dangers

Le coùt est-il un problème ? 
De nos jours, probablement que non. 
Cela ne devrait en général ne pas être un frein. 
Cette [page du site d'oracle](https://docs.oracle.com/javase/tutorial/essential/concurrency/immutable.html) exprime bien cela :
> Programmers are often reluctant to employ immutable objects, because they worry about the cost of creating a new object as opposed to updating an object in place. The impact of object creation is often overestimated, and can be offset by some of the efficiencies associated with immutable objects. These include decreased overhead due to garbage collection, and the elimination of code needed to protect mutable objects from corruption. 

Attention, cela ne veut pas dire que parfois avoir des objets mutables sera une meilleure option. 
C'est une question de compromis. 
Pour faire ce choix on pourra s'aider de quelques métriques (taille mémoire, vitesse d'exécution).

Le plsu gros danger que je vois est plus le suivant :
si le choix est appliqué de façon dogmatique, cela finirait par rendre le code plus complexe qu'il ne devrait l'être. 

## Comment rendre une classe immutable en Java ?

* on met la classe `final` ou alors (mieux), on crée un constructeur privé avec des *factory methods*. 
Pourquoi mieux ? 
Parce que l'on se réserve l'opportunité de sous classer en interne la classe
* pas de mutateur (*setter*)
* champs `final`
* si un a un champ non `final`, alors il ne faut pas l'exposer aux clients. 
Ce peut être le cas pour des champs couteux à instancier, que l'on va rendre *lazy*. 
* un constructeur qui vérifie les invariants (on peut s'aider par exemple de : `Objects.requireNonNull`)

### Mais si je dois créer un objet en plusieurs étapes

Si on est amené à valoriser les champs en plusieurs étapes, il faut avant tout investiguer si cela n'est pas un smell, si on n'aurait pas pu le faire en même temps que les autres. 
Si ce n'est pas possible, on peut explorer d'autres solutions : 
* l'objet offre une méthode qui va gérer toutes les opérations, et en interne on peut faire la mutation avec un compagnon mutable.
* on offre un compagnon mutable public (voir le pattern [Builder](https://refactoring.guru/design-patterns/builder) ou [Essence](https://wiki.c2.com/?EssenceObject)), comme c'est le cas pour `String` avec `StringBuilder`.
* on fait de la composition à partir de plusieurs objets immutables (et on délègue les messages vers ces objets), ou on peut recréer un nouvel objet complet une fois les différentes informations récupérées. 

On choisit la meilleure solution en fonction de chaque situation. 

## Intérêt du mutable
Et oui, il ne faut jamais être dogmatique, parfois ça vaut le coup de mettre de la mutabilité. 
Par exemple pour certains objets de communication en dehors de l’hexagone, nos DTOs (mappés avec Jackson, JPA, etc). 
Sur ce coup là on n'a pas vraiment le choix, et ça n'aurait pas beaucoup d'intérêt. 
Mais d'en d'autres cas, par exemple au sein d'une fonction, je peux créer une liste mutable pour me faciliter la mise en place d'un algorithme. 
Mais aussi, comme on l'a vu plus haut, si des impératifs de performance nous l'imposent et que des mesures nous montrent que cela est une amélioration, alors on peut choisir de partir sur de objets mutables. 

## Conclusion

La balance devrait donc souvent pencher en faveur du choix de l'immutablitité. 

> Classes should be immutable unless there’s a very good reason to make them mutable. If a class cannot be made immutable, limit its mutability as much as possible. *J.Bloch - Effective Java* 

Ou en français : 
> les classes devraient être immutables sauf s'il y a une bonne raison de les rendres mutables. Si une classe ne peut être immutable, limitez sa mutabilité le plus possible

## Ressource 
* le livre [effective Java](https://www.goodreads.com/book/show/34927404-effective-java)
* [site de yegor](https://www.yegor256.com/2014/06/09/objects-should-be-immutable.html)
* [le site d'oracle](https://docs.oracle.com/javase/tutorial/essential/concurrency/imstrat.html)
