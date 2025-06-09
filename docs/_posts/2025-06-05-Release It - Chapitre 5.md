---
layout: notes
title: Release It! - Chapitre 5
date: 2025-06-05 00:00:00 +0100
categories: notes_de_lecture
---
Je continue la lecture du livre [Release It!](https://pragprog.com/titles/mnee2/release-it-second-edition/) de Michael Nygard. 
Voici mes notes sur le chapitre 4, _Stability Patterns_. 

## Timeouts  
Placer des timeouts aux points d'intégration avec les autres systèmes est indispensable. 
On évite d'être contaminé par un problème extérieur. 

Au sein de notre système, cela est important aussi à certains endroits, comme un pool de ressources. 

> *Apply Timeouts to Integration Points, Blocked Threads, and Slow Responses.*
> The Timeouts pattern prevents calls to Integration Points from becoming Blocked Threads. Thus, timeouts avert Cascading Failures.

Toujours utiliser les méthodes avec timeout. 
Gérer un timeout ajoute de la complexité au code. 
Mais c'est un prix à payer. 
On utilise le plus possible des librairies qui simplifient la gestion des timeouts, ou on met en place nous même des primitives qui nous le permettent si nécessaire. 
Eviter les `synchronized` en Java. 
Les librairies devraient fournir uniquement des méthodes avec timeout. 

Si on arrive au timeout, alors il vaut mieux : 
* retourner un résultat immédiatement
* faire un retry, mais plus tard, car tout de suite il est fort probable que l'appel échoue à nouveau et que cela empir la situation

> If you cannot complete an operation because of some timeout, it is better for you to return a result.

Un ordre de grandeur : une API devrait répondre entre 10 et 100ms. 

> For a service behind a web API, “fast enough” is probably between 10 and 100 milliseconds. 
> Beyond that, you’ll start to lose capacity and customers.

## Circuit Breaker
Le _Circuit Breaker_ est inspiré des disjoncteurs électriques. 
Il permet de protéger un système en détectant automatiquement les défaillances d'intégration. 
Il arrête temporairement les appels vers des services défaillants pour éviter les pannes en cascade. 
Ce mécanisme "fail first" permet à un sous-système de tomber en panne sans détruire l'ensemble du système.

> Circuit Breaker is the fundamental pattern for protecting your system from all manner of Integration Points problems. When there’s a difficulty with Integration Points, stop calling it! 

Le circuit a 3 états. 
Au début le circuit est fermé, les appels passent. 
En cas d’échecs répétés le circuit s’ouvre. 
Les appels ne passent plus. 
Au bout d’un certain temps, on réessaie l’opération, dans un état “semi-ouvert”. 
Si elle échoue, on ouvre à nouveau le circuit. 
Sinon, on repart normalement, en le fermant. 

Quand le circuit est ouvert, on peut avoir différentes stratégies. 
Lancer une exception, retourner une réponse générique, ou la dernière valeur en cache, etc. 
C’est une décision à prendre avec les métier. 

> Therefore, it’s essential to involve the system’s stakeholders when deciding how to handle calls made when the circuit is open. 

L’activation d’un disjoncteur doit remonter une alarme et être suivie avec du monitoring pour analyser son activité. 

Il faut un _circuit breaker_ pour un process (= ne pas les réutiliser). 

Utiliser une lib. 

Les *circuit breakers* permettent de répondre aux antipatterns suivants (vus au chapitre précédent) : integration points, cascading failures, unbalanced capacities, slow responses.

## Steady State
Éviter autant que possible d’avoir à intervenir manuellement sur les serveurs. 

Pour éviter d’accumuler des données qui ne servent plus, purger les données dans l’application (c’est une connaissance métier). 

Limiter la taille des caches avant qu’ils ne prennent toute la mémoire. 

Utiliser la rotation des logs.

## Fail Fast
> If your system cannot meet its SLA, inform callers quickly. Don’t make them wait for an error message, and don’t make them wait until they time out.

> Do basic user input validation even before you reserve resources.

## Let It Crash
Dans certains cas, il est plus efficace de laisser le système s'écrouler. 
Mais pour que cela ait un intérêt il faut que le composant : 
* soit isolé des autres afin de ne pas entrainer une chute en cascade (par exemple avec un circuit breaker)
* soit très rapide à redémarrer

> Large processes with heavy runtimes or long startups are not the right place to apply this pattern. 
> Applications that couple many features into a single process are also a poor choice. 
