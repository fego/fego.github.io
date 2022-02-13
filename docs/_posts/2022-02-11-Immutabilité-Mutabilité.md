# (Draft) Immutabilité et mutabilité en Java, choisir en connaissance de cause

*Article en cours de rédaction*. 
*Temp de lecture : environ 7 mintues*. 

Pendant longtemps je n'ai vu sur mon chemin que des objets métiers mutables. 
Les fameux POJO, avec leurs *getters* et *setters* que l'on crée par mimétisme. 
C'était le cas bien souvent car nous n'avions qu'un même modèle du *controller* à la base de données.

Sur mon projet actuel nous essayons de passer sur une architecture hexagonale. 
Il y a beaucoup de travail car beaucoup de code a été produit avant que nous ne prenions cette direction. 
Une des tâches est d'introduire un modèle métier, indépendant de celui de l'API et celui de la persistance. 
Nous avons de plus décidé de partir sur un modèle immutable sous l'impulsion d'un collègue convaincu et convaincant. 
J'avoue au début avoir été sur la retenue. 
J'avais peur d'introduire trop de complexité, que cela nous ralentisse. 
Aujourd'hui, après quelques rectifications, le résultat est à mon avis vraiment positif. 

Je vous partage donc les apports de ce choix dans notre contexte. 
Rien de nouveau sur le sujet, c'est ma synthèse de ce que j'ai pu lire. 
J'espère vous intéresser si vous n'avez pas encore creusé ce sujet, et que vous évoluez dans l'écosystème Java. 

## Les bénéfices
En résumé : plus de lisibilité et moins de bugs. 
Voyons le détail.

### Atomicité de construction
Soit l’objet est créé, soit ça a planté, mais il ne peut être dans un état invalide. 
Ses invariants sont validés à la construction. 
C'est un **énorme** avantage. 
On peut utiliser l'objet sans crainte et s'éviter des vérifications fastidieuses, de la programmation défensive. 

```java
// Quantite.java
private final int val;
public Quantite(int val) {
    if (val < 0) {
        throw new IllegalArgumentException("La quantité doit être >= 0");
    }
    this.val = val;
}

// LigneDeCommande.java
private final long idArticle
private final Quantite quantite;
public LigneDeCommande(long idArticle, Quantite quantite) {
    this.idArticle = idArticle;
    this.quantite = Objects.requireNonNull(quantite, "une quantité est obligatoire");
}
```

En utilisant l'objet `LigneDeCommange`, je n'aurais jamais à me « méfier » de `quantite` : 
que ce soit en passant par un *getter* ou même dans une méthode interne, cet attribut existe forcément
(et représente une valeur qui a du sens fonctionnellement, cf. la contrainte de son constructeur). 

### Couplage temporel

Un couplage temporel, c'est quand des instructions doivent absolument être exécutées avant d'autres, sur une grappe d'objets. 
Si on ne respecte pas cet ordre, alors on peut rencontrer une exception ou être dans un état fonctionnellement invalide. 
Avec des objets immutables on ne peut séparer l’instanciation de l’initialisation, donc il ne sera possible d'utiliser l'objet avant de l'avoir initialisé. 

### Partageable sans crainte

Il n'y aura pas d'effets de bord. 
Avec l'immutabilité on n’aura jamais à investiguer où l’état a été modifié. 
On peut les réutiliser partout ou nécessaire, comme par exemple `BigDecimal.ZERO`. 
À l'inverse, l'utilisation d'un objet mutable peut créer des bugs difficiles à résoudre. 
Par exemple, modifier un objet mutable issu d'un cache corrompt le cache. 
Si on est obligé pour une raison quelconque d'avoir un objet mutable, il est alors conseillé de retourner une copie de cet objet. 

### _Thread safety_

Comme les soucis de concurrence ne concernent que *l'écriture* et qu'un objet immutable ne peut jamais être modifié, il peut être partagé sans crainte dans un contexte concurrent.

### Identité de l'objet
J'utilise le terme "identité" ici dans le sens de c'est ce qui est déterminé par `equals`et `hashcode` (l'harmonie des deux implémentations étant désirable). 
Par exemple dans l'ancienne API `java.util.Date` l'identité est déterminée par la comparaison du getter `getTime` : 

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
Date key = new Date();
map.put(key, "hello, world!");
key.setTime(12345L);
assert map.containsKey(key); 
```
La dernière instruction va échouer car le *hash* de la clé n'est plus le même qu'à l'insertion. 
Fort heureusement, la nouvelle api `java.time` est désormais immutable. 

## Les dangers

Le coût de création d'un objet est-il un problème ? 
De nos jours, probablement que non. 
Cela ne devrait en général ne pas être un frein. 
Cette [page du site d'oracle](https://docs.oracle.com/javase/tutorial/essential/concurrency/immutable.html) exprime bien cela :
> Programmers are often reluctant to employ immutable objects, because they worry about the cost of creating a new object as opposed to updating an object in place. The impact of object creation is often overestimated, and can be offset by some of the efficiencies associated with immutable objects. These include decreased overhead due to garbage collection, and the elimination of code needed to protect mutable objects from corruption. 

ou en français : 

> Les developpeurs sont souvent réticents à utiliser des objets immuables, car ils s'inquiètent du coût de la création d'un nouvel objet par opposition à la mise à jour d'un objet déjà existant. L'impact de la création d'objets est souvent surestimé et peut être compensé par des gains d'efficacité associés aux objets immuables. Celles-ci incluent une diminution du surcoût due au passage du *garbage collector* et l'élimination du code nécessaire pour protéger les objets mutables de la corruption.

Attention, cela ne veut **pas** dire qu'avoir des objets mutables n'est *jamais* une meilleure option. 
C'est une question de compromis. 
Pour faire ce choix on pourra s'aider de quelques métriques (taille mémoire, vitesse d'exécution). 

Le plus gros danger que je vois est plutôt le suivant :
si le choix est appliqué de façon dogmatique, cela finira par rendre le code plus complexe qu'il ne devrait l'être. 
Sur notre projet, nous n'avons pas pris les bonnes options sur certains objets, et leur l'usage en est devenu complexe. 
Le moment particulièrement sensible que nous avons rencontré était quand un concept nécessitait plusieurs étapes pour être construit. 
Les solutions à ce problème sont décrites plus bas. 
Mais tout d'abord, voyons comment rendre un objet immutable. 

## Comment rendre une classe immutable en Java ?

* pas de mutateur (*setter*)
* on met tous les champs `final`
* si un a un champ non `final`, alors il ne faut pas l'exposer aux clients. 
Ce peut être le cas pour des champs couteux à instancier, que l'on va rendre *lazy*. 
* un constructeur qui vérifie les invariants (on peut s'aider par exemple de : `Objects.requireNonNull`)
* il faut que les classes qui héritent continuent de garantir l'immutabilité. 

À partir de Java 16 on peut aussi utiliser des *[records](https://docs.oracle.com/en/java/javase/17/language/records.html)* pour créer simplement des classes immutables, sans *boilerplate*. 

### Note sur l'héritage

Déclarer la classe `final` permet de garantir qu'une classe fille ne viendra pas corrompre l'immutabilité.

L'autre solution est de créer un constructeur privé et des *factory methods*.
Ceci nous donne l'avantage de sous-classer en interne. 

Note : en Java 17 le mot clé [`sealed`](https://docs.oracle.com/en/java/javase/17/language/sealed-classes-and-interfaces.html) a été introduit et permet de déterminer les classes qui ont le droit d'hériter de la classe, permettant de décrire des classes filles dans un autre fichier. 

### Le diable se cache dans les détails

Attention, ce n'est pas parce que l'on ajoute `final` à ses attribut que notre objet sera immutable ! 

Par exemple :
```java
final class Strings implements Iterable<String> {
    private final List<String> val;
    public Strings(List<String> val) {
        this.val = new ArrayList<>(val);
    }

    @Override
    public Iterator<String> iterator() { return val.iterator(); }
}
```
Où on a même pris soin de se défendre contre la modification ultérieure de la liste fournie à l'initialisation et de ne pas exposer directement notre unique attribut.
Néanmoins cet objet n'est **pas** immutable :
```java
class Mutator {
    public Optional<String> pop(Strings queue) {
        Iterator<String> i = queue.iterator();
        if (i.hasNext()) {
            String val = i.next();
            i.remove(); // 💔 mutation quand même
            return Optional.of(val);
        }
        return Optional.empty();
    }
}
```

Le choix de la solution sera guidé par ce qu'on préfère :
* _fail-fast_
  * tout usage « impropre » de l'instance jetera une exception
  * utiliser `List.copyOf` dans le constructeur de `Strings`
* _idempotence_
  * garantir que la donnée ne bouge pas est suffisant
  * utiliser une copie de l'attribut à chaque fois qu'on doit le communiquer (même indirectement, comme ici)

### Mais si je dois créer un objet en plusieurs étapes

Si on est amené à valoriser les champs en plusieurs étapes, il faut avant tout investiguer si cela n'est pas un *smell*, si on n'aurait pas pu le faire en même temps que les autres. 
Si ce n'est pas possible, on peut explorer d'autres solutions : 
* l'objet offre une méthode qui va gérer toutes les opérations, et en interne on peut faire la mutation avec un compagnon mutable.
* on offre un compagnon mutable public (voir le pattern [Builder](https://refactoring.guru/design-patterns/builder) ou [Essence](https://wiki.c2.com/?EssenceObject)), comme c'est le cas pour `String` avec `StringBuilder`.
* on fait de la composition à partir de plusieurs objets immutables (et on délègue les messages vers ces objets), ou on peut recréer un nouvel objet complet une fois les différentes informations récupérées. 

On choisit la meilleure solution en fonction de chaque situation. 
Vous constaterez que dans certains cas vous écrirez plus de code, par exemple lors de l'ajout d'un _builder_. 
C'est le prix de la qualité, qui sera amorti par la clarté d'usage pour les developpeurs qui s'en serviront, vous-du-futur inclu !
On peut citer ici la disjonction entre les méthodes d'enrichissement (uniquement dans le _builder_) et les méthodes d'utilisation de la donnée (uniquement dans l'objet immuable).

## Intérêt du mutable
Car il ne faut jamais être dogmatique, parfois ça vaut le coup de mettre de la mutabilité. 
Par exemple pour certains objets de communication en dehors de l’hexagone, nos DTOs (mappés avec Jackson, JPA, etc). 
Sur ce coup là on n'a pas vraiment le choix, et ça n'aurait pas beaucoup d'intérêt. 
Mais dans d'autres cas, par exemple au sein d'une fonction, je peux créer une liste mutable pour me faciliter la mise en place d'un algorithme. 
Mais aussi, comme on l'a vu plus haut, si des impératifs de performance nous l'imposent et que des mesures nous montrent que cela est une amélioration, alors on peut choisir de partir sur de objets mutables. 

## Conclusion

J'espère que cet article vous aura donné quelques éléments vous permettant de considérer de rendre vos objets immutables quand cela vous paraitra pertinent. 

> Classes should be immutable unless there’s a very good reason to make them mutable. If a class cannot be made immutable, limit its mutability as much as possible. *J.Bloch - Effective Java* 

Ou en français : 
> les classes devraient être immutables sauf s'il y a une bonne raison de les rendre mutables. Si une classe ne peut être immutable, limitez sa mutabilité le plus possible

*Un grand merci aux relecteurs des [Software Cratfers](https://www.meetup.com/fr-FR/nantes-software-crafters-Nantes/) de Nantes :)*

## Ressources (en anglais)
* le livre [effective Java](https://www.goodreads.com/book/show/34927404-effective-java)
* une [page sur l'immutabilité](https://www.yegor256.com/2014/06/09/objects-should-be-immutable.html) de Yegor Bugayenko
* *A Strategy for Defining Immutable Objects* sur [le site d'oracle](https://docs.oracle.com/javase/tutorial/essential/concurrency/imstrat.html)
* [un bout de cours du MIT](https://web.mit.edu/6.005/www/fa15/classes/09-immutability/) sur l'immutabilité. 
* [un article intéressant](https://blogs.oracle.com/javamagazine/post/java-immutable-objects-strings-date-time-records) avec un peu d'historique sur les décisions
