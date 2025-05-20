---
layout: notes
title: Release It! - Chapitre 4 - Stability Antipatterns - Integration Points
date: 2025-05-20 00:00:00 +0100
categories: notes_de_lecture
---
Je continue la lecture du livre [Release It!](https://pragprog.com/titles/mnee2/release-it-second-edition/) de Michael Nygard. 
Voici mes notes sur le chapitre 4, _Stability Antipatterns_. 
Ce chapitre étant long, je découpe ma lecture par sous parties. 
Aujourd'hui la section _Integration Points_. 

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