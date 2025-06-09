---
layout: notes
title: Release It! - Chapitre 4
date: 2025-05-20 00:00:00 +0100
categories: notes_de_lecture
---
Je continue la lecture du livre [Release It!](https://pragprog.com/titles/mnee2/release-it-second-edition/) de Michael Nygard. 
Voici mes notes sur le chapitre 4, _Stability Antipatterns_. 

## Integration points
Les projets tombent globalement dans 2 catégories. 
Le papillon ou la toile. 
Le papillon c’est un système central, un monolithe, connecté à d’autres systèmes, avec beaucoup d’appelants, beaucoup d’appelés.  
L’autre style, la toile, est un ensemble de systèmes interconnectés. 
Chaque connexion entre 2 systèmes est un point d’intégration et peut détruire le système. 
Plus on a de petits services, plus on s’intègre avec d’autres systèmes, plus c’est risqué. 

> In fact, the more we move toward a large number of smaller services, the more we integrate with SaaS providers, and the more we go API first, the worse this is going to get.

> Integration points are the number-one killer of systems.

Parfois, quand on établit une connexion avec un autre système, on est rejeté tout de suite, mais parfois la connexion va être bloquée jusqu’au *read timeout*. 
Ça peut être long (et parfois il n’y en a pas de configuré). 

Et parfois quand ça casse, il faut mettre les mains dans le cambouis, dans du code que l’on n’a pas écrit. 

> [...] not every problem can be solved at the level of abstraction where it manifests. 
> Sometimes the causes reverberate up and down the layers. 
> You need to know how to drill through at least two layers of abstraction to find the “reality” at that level in order to understand problems. 

Il faut utiliser une librairie qui nous permet de paramétrer les timeouts. 
Il suggère d’éviter les librairies qui font le mapping de la réponse vers des objets. 
Il revient sur ce sujet au chapitre 11. Mon expérience personnelle me montre que le défaut d’une API comme Feign (qui fait ce type de mapping) c’est de donner la sensation à l’utilisateur que tout va bien se passer. 
Il peut passer complètement à côté de la gestion des exceptions. 
Mais j’imagine qu’il y a d’autres raisons, et je suis curieux d’en savoir plus. 

> Use a client library that allows fine-grained control over timeouts—including both the connection timeout and read timeout—and response handling. 
> I recommend you avoid client libraries that try to map responses directly into domain objects. 
> Instead, treat a response as data until you’ve confirmed it meets your expectations. 
> It’s just text in maps (also known as dictionaries) and lists until you decide what to extract.

Les patterns pour amener de la stabilité sont *circuit breaker* et *decoupling middleware* abordés après. 

Et il faut tester aussi chaque point d’intégration. 
On simule les partenaires, et on peut simuler toutes sortes d’erreurs. 

> To make sure your software is cynical enough, you should make a test harness—a simulator that provides controllable behavior—for each integration test. […] 
> Setting the test harness to spit back canned responses facilitates functional testing. It also provides isolation from the target system when you’re testing. Finally, each such test harness should also allow you to simulate various kinds of system and network failures.

## Chain Reactions & Cascading failures
La distinction entre une réaction en chaîne et des échecs en cascade est la suivante. 
Dans une réaction en chaîne, c’est une propagation horizontale, alors que l’échec en cascade est une propagation verticale. 
Une réaction en chaîne peut entraîner des échecs en cascade. 

Les contre-mesures : 
* _Cascading Failures_ : Timeouts stricts + Circuit Breaker pour couper rapidement les appels vers la couche défaillante. 
* _Chain Reactions_ : Chasser et corriger les fuites de mémoire et les bugs de concurrence. 
Utiliser le pattern _Bulkheads_ (à voir plus tard) pour cloisonner en sous-groupes. 
Autoscaling avec health checks pour remplacer vite les nœuds tombés. 

Différents profils d'utilisateurs peuvent amener différents problèmes. 
Il y a par exemple la simple hausse de trafic, à contrebalancer par de l'autoscaling (mais attention à la facture, certaines applications avec des problèmes peuvent être très coûteuses). 
On peut utiliser aussi des mémoires externes comme Redis pour alléger notre serveur de cette responsabilité. 
Il y a les utilisateurs qui vont sur les pages les plus coûteuses, et qui sont souvent les utilisateurs qui rapportent de l'argent. 
Il faut se préparer par des tests. 

> The best thing you can do about expensive users is test aggressively. Identify whatever your most expensive transactions are and double or triple the proportion of those transactions

## Blocked Threads
Les threads bloqués menacent la stabilité d'une application. 
Cela arrive souvent avec les pools de ressources, comme les pools de connexions à la base de données. 
On évite de recréer soi-même du code pour gérer les pools de connexions. 
On utilise des libs existantes qui ont fait leur preuves.  
Un timeout doit toujours être configuré quand on acquiert une ressource. 

## Self Denial Attacks
Il peut arriver aussi que l'on mette en danger son propre système sans le vouloir. 
Par exemple une campagne commerciale qui va s'emballer et générer un énorme trafic qui fera tomber les serveurs. 

> Send mass emails in waves to spread out the peak load. 
> Create static “landing zone” pages for the first click from these offers. 
> Watch out for embedded session IDs in URLs. 

## Slow Responses
> generating a slow response is worse than refusing a connection or returning an error, particularly in the context of middle-layer services. 

Une réponse lente peut entraîner des échecs en cascade dans un système. 
Cela peut aussi augmenter trafic, les utilisateurs impatients rechargeant les pages. 
Il vaut mieux échouer rapidement si on dépasse un certain seuil. 

## Unbounded Result Sets
Il faut aussi faire attention à la taille de données que l'on manipule. 
Au départ on peut avoir un volume réduit, mais celui-ci peut croître de façon innattendue. 
Il faut limiter la taille des résultats. 

En dev, on utilise des volumes de données réalistes. 

> In any API or protocol, the caller should always indicate how much of a response it’s prepared to accept.

[Notes de le suite du livre]({% link _posts/2025-06-05-Release It - Chapitre 5.md %})