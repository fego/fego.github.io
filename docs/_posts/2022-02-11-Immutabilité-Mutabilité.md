# (Draft) Immutabilité et mutabilité en Java, choisir en connaissance de cause

*Article en cours de rédaction*. 

Pendant longtemps je n'ai vu sur mon chemin que des objets métiers mutables. 
Les fameux POJO, avec leurs *getters* et *setters* que l'on crée par mimétisme. 
C'était le cas bien souvent car nous n'avions qu'un même modèle du *controller* à la base de données. 
Sur mon projet actuel nous essayons de passer sur une architecture hexagonale. 
Il y a beaucoup de travail car beaucoup de code a été produit avant que nous ne prenions cette direction. 
Une des tâches est d'introduire un modèle métier, indépendant de celui de l'api et celui de la base. 
Et nous avons décidé de partir sur un modèle immutable sous l'impulsion d'un collègue convaincu et convaincant. 
J'avoue au début avoir été sur la retenue. 
J'avais peur que nous introduisions trop de complexité, que cela nous ralentisse. 
Aujourd'hui, après quelques rectifications, le résultat est à mon avis vraiment positif. 
Je vous partage donc ce qu'aujourd'hui ce modèle nous a apporté dans notre contexte. 
Rien de nouveau sur le sujet, c'est ma synthèse de ce que j'ai pu lire. 
J'espère vous intéresser si vous n'avez pas encore creusé ce sujet, et que vous évoluez dans l'écosystème Java. 

## Les bénéfices
En résumé : on a plus de lisibilité et moins de bugs. 
Voyons le détail.

### Atomicité de construction
Soit l’objet est créé, soit ça a planté, mais il ne peut être dans un état invalide. 
Ses invariants sont validés à la construction. 
C'est un **énorme** avantage. 
On peut utiliser l'objet sans crainte et s'éviter des vérifications fastidieuses, de la programmation défensive. 

```java
public LigneDeCommande(Long idArticle, Integer quantite) {
    this.idArticle = Objects.requireNonNull(idArticle);
    if (quantite < 0) {
        throw new IllegalArgumentException("La quantite doit être >= 0");
    }
    this.quantite = Objects.requireNonNull(quantite);
}
```

En utilisant cet objet, je n'aurais jamais à vérifier si la `quantite` est `null`, que ce soit en passant par un *getter* ou même dans une méthode interne. 

### Couplage temporel

Un couplage temporel, c'est quand des instructions doivent être absolument exécutées dans un ordre précis. 
Si on ne respecte pas cet ordre, alors on peut rencontrer une exception ou être dans un état fonctionnellement invalide. 
Avec des objets immutables on ne peut séparer l’instanciation de l’initialisation, donc il ne sera possible d'utiliser l'objet avant de l'avoir initialisé. 

### Partageable sans crainte

Il n'y aura pas d'effets de bord. 
Avec l'immutabilité on n’aura jamais à investiguer où l’état a été modifié. 
On peut les réutiliser partout ou nécessaire, comme par exemple `BigDecimal.ZERO`. 
A l'inverse avec un objet mutable on peut créer des bugs difficiles à résoudre. 
Par exemple un objet mutable issu d'un cache, si on le modifie on corrompt le cache. 
Si on est obligé pour une raison quelconque d'avoir un objet mutable, il est alors conseillé de retourner une copie de cet objet. 

### Thread safety

Avec de l'immutabilité, on s'enlève de la complexité dans la gestion des accès concurrents et des bugs très difficiles à débugger puisqu'un objet immutable ne pourra pas être modifié par plusieurs threads en parallèle.

### Identité de l'objet
J'utilise le terme "identité" ici dans le sens de c'est ce qui est déterminé par `equals`et `hashcode` (l'implémentation de l'un impliquant l'implémentation de l'autre). 
Par exemple dans l'ancienne api `java.util.Date` l'identité est déterminée par la comparaison du getter `getTime` : 

```java
public boolean equals(Object obj) {
    return obj instanceof Date && getTime() == ((Date) obj).getTime();
}

public int hashCode() {
    long ht = this.getTime();
    return (int) ht ^ (int) (ht >> 32);
}
```

Or, cet objet étant mutable, le changer, c'est modifier son identité. 
Voici un exemple issu d'un article de Yegor Bugayenko. 

```java
Map<Date, String> map = new HashMap<>();
Date date = new Date();
// ici la clé de la HashMap est calculée avec hashCode, donc à partir de la valeur de getTime à cet instant
map.put(date, "hello, world!");
// en modifiant la date, la valeur retournée par hashCode n'est plus la même
date.setTime(12345L);
// false car le hash de la clé n'est plus le même que le hash de la date. 
assert map.containsKey(date); 
```
Et oui, le hash de la data n'est plus le même. 
Fort heureusement, la nouvelle api `java.time` est désormais immutable. 

## Dangers

Le coût est-il un problème ? 
De nos jours, probablement que non. 
Cela ne devrait en général ne pas être un frein. 
Cette [page du site d'oracle](https://docs.oracle.com/javase/tutorial/essential/concurrency/immutable.html) exprime bien cela :
> Programmers are often reluctant to employ immutable objects, because they worry about the cost of creating a new object as opposed to updating an object in place. The impact of object creation is often overestimated, and can be offset by some of the efficiencies associated with immutable objects. These include decreased overhead due to garbage collection, and the elimination of code needed to protect mutable objects from corruption. 

ou en français : 

> Les programmeurs sont souvent réticents à utiliser des objets immuables, car ils s'inquiètent du coût de la création d'un nouvel objet par opposition à la mise à jour d'un objet en place. L'impact de la création d'objets est souvent surestimé et peut être compensé par des gains d'efficacité associés aux objets immuables. Celles-ci incluent une diminution du surcoût due au passage du garbage collector et l'élimination du code nécessaire pour protéger les objets mutables de la corruption.

Attention, cela ne veut pas dire que parfois avoir des objets mutables sera une meilleure option. 
C'est une question de compromis. 
Pour faire ce choix on pourra s'aider de quelques métriques (taille mémoire, vitesse d'exécution). 

Le plus gros danger que je vois est plutôt le suivant :
si le choix est appliqué de façon dogmatique, cela finira par rendre le code plus complexe qu'il ne devrait l'être. 
Sur notre projet, nous avons été par moment trop loin, ou de façon maladroite. 
Le moment particulièrement sensible que nous avons rencontré était quand un concept nécessitait plusieurs étapes pour être construit. 
J'adresse ce point plus bas. 
Mais tout d'abord, voyons comment rendre un objet immutable. 

## Comment rendre une classe immutable en Java ?

* pas de mutateur (*setter*)
* on met tous les champs `final`
* si un a un champ non `final`, alors il ne faut pas l'exposer aux clients. 
Ce peut être le cas pour des champs couteux à instancier, que l'on va rendre *lazy*. 
* un constructeur qui vérifie les invariants (on peut s'aider par exemple de : `Objects.requireNonNull`)
* il faut que les classes qui héritent continuent de garantir l'immutabilité. 
S'il y a un risque que cela ne soit pas le cas, il est recommandé de mettre la classe `final` ou alors (mieux), on crée un constructeur privé avec des *factory methods*. 
Pourquoi mieux ? 
Parce que l'on se réserve l'opportunité de sous classer en interne la classe. 
Note : en Java 17 le mot clé [`sealed`](https://docs.oracle.com/en/java/javase/17/language/sealed-classes-and-interfaces.html) a été introduit et permet de déterminer les classes qui ont le droit d'hériter de la classe. 

Attention, ce n'est pas parce que l'on ajoute `final` que le type sera immutable. 
C'est bien la référence que l'on ne pourra pas changer. 
Par exemple pour une liste, si on n'utilise pas une liste immutable on aura beau la déclarer en `final` cette liste sera modifiable. 
Il faut par exemple utiliser les différentes surcharges de `List.of()`. 

A partir de Java 16 on peut aussi utiliser des *[records](https://docs.oracle.com/en/java/javase/17/language/records.html)* pour créer simplement des classes immutables, sans *boilerplate*. 

### Mais si je dois créer un objet en plusieurs étapes

Si on est amené à valoriser les champs en plusieurs étapes, il faut avant tout investiguer si cela n'est pas un smell, si on n'aurait pas pu le faire en même temps que les autres. 
Si ce n'est pas possible, on peut explorer d'autres solutions : 
* l'objet offre une méthode qui va gérer toutes les opérations, et en interne on peut faire la mutation avec un compagnon mutable.
* on offre un compagnon mutable public (voir le pattern [Builder](https://refactoring.guru/design-patterns/builder) ou [Essence](https://wiki.c2.com/?EssenceObject)), comme c'est le cas pour `String` avec `StringBuilder`.
* on fait de la composition à partir de plusieurs objets immutables (et on délègue les messages vers ces objets), ou on peut recréer un nouvel objet complet une fois les différentes informations récupérées. 

On choisit la meilleure solution en fonction de chaque situation. 
Vous constaterez que dans certains cas nous avons plus de code, par exemple lors de l'ajout d'un builder. 
C'est est le prix de la qualité, tout comme un pour langage statiquement typé la verbosité sera la coût de la qualité. 

## Intérêt du mutable
Et oui, il ne faut jamais être dogmatique, parfois ça vaut le coup de mettre de la mutabilité. 
Par exemple pour certains objets de communication en dehors de l’hexagone, nos DTOs (mappés avec Jackson, JPA, etc). 
Sur ce coup là on n'a pas vraiment le choix, et ça n'aurait pas beaucoup d'intérêt. 
Mais d'en d'autres cas, par exemple au sein d'une fonction, je peux créer une liste mutable pour me faciliter la mise en place d'un algorithme. 
Mais aussi, comme on l'a vu plus haut, si des impératifs de performance nous l'imposent et que des mesures nous montrent que cela est une amélioration, alors on peut choisir de partir sur de objets mutables. 

## Conclusion

J'espère que cet article vous aura donné quelques éléments vous permettant de considérer de rendre vos objets immutables quand cela vous paraitra pertinent. 

> Classes should be immutable unless there’s a very good reason to make them mutable. If a class cannot be made immutable, limit its mutability as much as possible. *J.Bloch - Effective Java* 

Ou en français : 
> les classes devraient être immutables sauf s'il y a une bonne raison de les rendres mutables. Si une classe ne peut être immutable, limitez sa mutabilité le plus possible

*Un grand merci aux préciaux relecteurs des [Softaware Cratfers](https://www.meetup.com/fr-FR/nantes-software-crafters-Nantes/) de Nantes :)*

## Ressource (en anglais)
* le livre [effective Java](https://www.goodreads.com/book/show/34927404-effective-java)
* [site de yegor](https://www.yegor256.com/2014/06/09/objects-should-be-immutable.html)
* [le site d'oracle](https://docs.oracle.com/javase/tutorial/essential/concurrency/imstrat.html)
* [un bout de cours du MIT](https://web.mit.edu/6.005/www/fa15/classes/09-immutability/) sur l'immutabilité. 
* [un article intéressant](https://blogs.oracle.com/javamagazine/post/java-immutable-objects-strings-date-time-records) avec un peu d'historique sur les décisions