---
layout: notes
title: Release It! - Chapitre 3
date: 2025-05-19 00:00:00 +0100
categories: notes_de_lecture
---
Je continue la lecture du livre [Release It!](https://pragprog.com/titles/mnee2/release-it-second-edition/) de Michael Nygard. 
Voici mes notes sur le chapitre 3, _Stabilize your System_. 

Nous devons construire un système stable, qui ne tombe pas quand il y a des stress (par exemple  un pic de charge, ou un composant tiers qui lag). 

> The amazing thing is that the highly stable design usually costs the same to implement as the unstable one.  

> A robust system keeps processing transactions, even when transient impulses, persistent stresses, or component failures disrupt normal processing. 

Les 2 gros dangers d’un système : les fuites de mémoire, et la croissance de données. 
Ces problèmes sont très durs à tester. 
Dans l’idéal il faut mettre en place un test de charge, juste sur une machine, et dans la durée. 

> The only way you can catch them before they bite you in production is to run your own longevity tests. 
> If you can, set aside a developer machine. 
> Have it run JMeter, Marathon, or some other load-testing tool. 
> Don’t hit the system hard; just keep driving requests all the time.  
> (Also, be sure to have the scripts slack for a few hours a day to simulate the slow period during the middle of the night. 
> That will catch connection pool and firewall timeouts.)

> Sometimes the economics don’t justify setting up a complete environment. 
> If not, at least try to test important parts while stubbing out the rest. 
> It’s still better than nothing. 

Sinon, c’est l’environnement de production qui devient l’environnement de test. 

Si on accepte que notre système va échouer, alors on a la possibilité de le concevoir pour anticiper les pannes à venir. 

Il revient sur l’incident du premier chapitre. 
En gros ils auraient pu éviter le pb en intervenant à différents niveaux. 
Notamment au niveau du pool de connexion, ils auraient pu créer plus de threads si le pool était plein, et aussi paramétrer un timeout. 

Un petit point terminologie : 
- *fault*, défaut. 
Une condition qui entraîne un état incorrect du système. 
Par exemple un bug qui attend son heure pour se révéler.
- *error*. 
Un comportement incorrect visible.
- *failure,* échec. 
Un système qui ne répond plus.

Un défaut est le début, le déclencheur qui peut aboutir à l’échec du système. 
Plus le système est couplé, plus il y a de chemins pour que cela arrive.

Une façon de se préparer aux échecs est de regarder tous les appels vers des dépendances externes, toutes les entrées sorties, et se demander ce qui pourrait ne pas marcher. 
Voici une liste de question qu’il suggère de se poser :

> - What if it can’t make the initial connection?
> - What if it takes ten minutes to make the connection?
> - What if it can make the connection and then gets disconnected?
> - What if it can make the connection but doesn’t get a response from the other end?
> - What if it takes two minutes to respond to my query?
> - What if 10,000 requests come in at the same time?
> - What if the disk is full when the application tries to log the error message about the SQLException that happened because the network was bogged down with a worm?

Il y a deux camps sur la façon de traiter les défauts. 
Un camp qui pense qu’il faut avoir un système tolérant à la panne, un autre qui pense qu’il y aura toujours des erreurs et qu’il faut laisser le système se vautrer de façon à repartir d’un état stable. 
Mais les deux camps sont en accord sur le fait que des défauts surviendront toujours, et qu’il faut éviter qu’ils deviennent des erreurs. 

[Notes de le suite du livre]({% link _posts/2025-05-20-Release It - Chapitre 4.md %})