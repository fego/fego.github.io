---
layout: notes
title: Conférence d'Alexis King - The unreasonable effectiveness of constructive data modeling. 
date: 2026-07-31 00:00:00 +0100
categories: notes_de_lecture
---
Au détour d'une errance sur Slack, je suis tombé sur [cette conférence](https://www.youtube.com/watch?v=0BXuYlNrUmE) d'Alexis King. 
Après [_parse don't validate_](https://lexi-lambda.github.io/blog/2019/11/05/parse-don-t-validate/), voici une nouvelle claque pour moi. 

Je vous partage mes notes. 
Ce n'est pas complet sur tous les points, mais ça couvre une bonne partie de la conférence. 
Vous pouvez consulter les slides [ici](https://github.com/lexi-lambda/talks/blob/master/2026-07%20constructive%20data%20modeling/slides.pdf). 

Tout d'abord, pour savoir si ce talk s'applique au langage que vous utilisez, il faut qu'il ait les caractéristiques suivantes : 
* une certaine forme de type produit. En Java des records. 
* une certaine forme de type sum. En Java, des sealed interfaces. 
* et aussi du pattern matching exhaustif (avec validation de la présence de toutes les variantes par le compilateur)

Si on a un langage qui supporte ces concepts, on peut faire ce qu'elle propose. 
En Java, c'est donc possible depuis la version 21. 

Je mets les exemples de code en Java, qui sont une interprétation des slides. 

## Ajouter plutôt que restreindre
En général, on voit les types comme des moyens de restreindre, d'apporter des contraintes. 
Mais on peut aussi plutôt construire de façon positive l'espace des données possibles. 

Par exemple, si on veut construire un type exprimant une liste non vide, on va avoir tendance naturellement à créer un type avec un champ de type `List` et vérifier dans le constructeur si la liste a une taille supérieure à 0. 

```java
record NonEmptyList<T>(List<T> items) {
  public NonEmptyList {
	  if (items.isEmpty()) throw new IllegalArgumentException();
  }
}
```
**Mais il reste des chemins pour casser l'invariant** ! 
Une lib comme Jackson pourrait passer par la réflexion et valoriser directement la liste, et donc contourner l'invariant. 
Aussi, si on passe une liste mutable, et que cette liste est vidée ailleurs dans le code après la construction, l'invariant n'est pas revalidé, et l'état devient invalide. 

L'alternative qu'elle propose, c'est de créer un type de deux champs : le premier élément de la liste, suivi d'une liste avec le reste. On sait qu'il y a au moins un élément et peut-être d'autres. 

```java
record NonEmptyList<T>(T head, List<T> tail) {}
```
Là on est détendu, pas de piège possible. 
**Il n'y a pas de chemin, aucun, qui permette de construire une valeur fausse.** 

A noter : en implémentant un record `NonEmptyList` plutôt que d'utiliser `List` on gagne sur la vérification de l'invariant, mais on perd l'accès aux méthodes de `List` (size, add, get, etc). Il faudra ajouter des méthodes qui délèguent au besoin. C'est un choix, un compromis, souvent intéressant. 

Autre exemple : on a un utilisateur, qui a des infos de contact comme un email ou un numéro de téléphone. L'utilisateur peut avoir soit un email, soit un numéro de téléphone, soit les deux mais ne peut pas en avoir aucun des deux. Pour modéliser ça, ce que l'on ferait en général : 
```java
record User(
    int id,
    Optional<EmailAddress> email,
    Optional<PhoneNumber> phone
) {}
// avec un contrôle dans le constructeur de l'invariant
```

Ce que l'on peut faire plutôt: 
```java
record User(int id, UserContact contact) {}

sealed interface UserContact
    permits UserContact.Email, UserContact.Phone, UserContact.Both {

    record Email(EmailAddress email) implements UserContact {}
    record Phone(PhoneNumber phone) implements UserContact {}
    record Both(EmailAddress email, PhoneNumber phone) implements UserContact {}
}
```

Et si on veut ajouter la possibilité d'être un utilisateur système plutôt, sans info de contact, en le faisant à posteriori, comment faire ? 
La mauvaise idée : 
```java
// on met l'invariant dans le constructeur
record User(
    int id,
    Optional<UserContact> contact,
    boolean isSystemUser
) {}

sealed interface UserContact
    permits UserContact.Email, UserContact.Phone, UserContact.Both {

    record Email(EmailAddress email) implements UserContact {}
    record Phone(PhoneNumber phone) implements UserContact {}
    record Both(EmailAddress email, PhoneNumber phone) implements UserContact {}
}
```
Ici rien n'empêche d'écrire `new User(1, Optional.empty(), false)`. 
Certes, ça va planter si on écrit une règle avec un if et un throw, mais le client peut écrire le code qui amène au plantage. Il faut un test pour s'en rendre compte, ça plante à l'exécution. 
Ci-dessous, on ne peut qu'écrire un code valide : 
```java
record User(int id, UserContact contact) {}

sealed interface UserContact
    permits UserContact.System, UserContact.Email, UserContact.Phone, UserContact.Both {

    record System() implements UserContact {}
    record Email(EmailAddress email) implements UserContact {}
    record Phone(PhoneNumber phone) implements UserContact {}
    record Both(EmailAddress email, PhoneNumber phone) implements UserContact {}
}
```

Elle donne un autre exemple : si on veut modéliser un intervalle de temps, avec un départ et une arrivée, voici ce que l'on ferait en général, pour éviter l'existence d'intervalles négatifs : 
```java
record TimeRange(Instant start, Instant end) {
    public TimeRange {
        if (start.isAfter(end)) throw new IllegalArgumentException();
    }
}
```
Mais on peut en fait le représenter comme cela : 
```java
record TimeRange(Instant start, Duration duration) {}
```
Note : dans la première représentation, le `if` n'a de sens que si on accède ensuite à la durée, en faisant `end - start`. Si on n'y accède jamais, le contrôle n'est pas nécessaire et la représentation est ok. 

Pour Alexis King, un système de type c'est pour **pouvoir suivre tous les cas qu'elle doit gérer sans peine, suivre les obligations** (_obligation propagation machine_).
Il permet de relier les endroits où on construit les valeurs avec les endroits où on va les utiliser. Par exemple, avec du pattern matching, on va être bloqué par le compilateur si on ajoute un type qui n'a pas été géré. Et les usages peuvent être très éloignés dans le code de la création. 

Mais attention à ne pas construire un type plus complexe que nécessaire. On prend la représentation la plus simple qui permet d'éviter d'ajouter des ifs ou de lancer des exceptions. 

Par exemple ici pas la peine d'inventer un type, tout marche bien : 
```java
double calculateTotal(List<InventoryLogEntry> entries) {
    return entries.stream()
        .mapToDouble(InventoryLogEntry::change)
        .sum();
}
```
Mais là, ça dérape, on doit lancer une exception pour le cas où on n'a pas d'élément : 
```java
Instant getLastChanged(List<InventoryLogEntry> entries) {
    return entries.stream()
        .findFirst()
        .map(InventoryLogEntry::timestamp)
        .orElseThrow(() -> new IllegalStateException("shouldn't happen"));
}
```
Ce qui serait mieux : 
```java
Instant getLastChanged(NonEmptyList<InventoryLogEntry> entries) {
    return entries.head().timestamp();
}
```

Ce qui se résume en fait à dire qu'il faut écrire des **fonctions totales**, des fonctions qui vont retourner systématiquement une valeur pour tous les cas possibles des paramètres. Au contraire d'une fonction partielle qui peut avoir des exceptions (pas de réponse pour des combinaisons de paramètres). 

Typer correctement, ça permet de mettre les bonnes responsabilités au bon endroit et de ne pas prendre des responsabilités qu'on ne devrait pas prendre. 
Par exemple si dans une signature on accepte un paramètre nullable, ou un `Optional`, alors on va devoir gérer ces cas dans la méthode. 
Alors qu'il faut peut-être que ce soit l'appelant qui ait cette responsabilité. 

Typer correctement, ça ne veut pas dire surtyper donc. 
Par exemple, dans certains cas, elle met juste une chaîne pour typer une adresse mail qu'elle va ensuite passer à son service qui envoie les e-mails. Dans son cas mettre un type `EmailAddress` ça ne vaut pas le coût, car à aucun moment elle n'a besoin de vérifier la validité (pas de if / throw). Elle a probablement une adresse issue d'une source qui garantit la validité (ou peut-être que même si l'adresse était ko, cela ne changerait rien à son métier). 

Et une dernière chose. 
Un type n'a pas forcément qu'une seule interprétation. Si on a une liste avec des paires dans sa représentation, on peut très bien fournir les éléments un par un aux clients. On découple la représentation de l'interprétation. 
