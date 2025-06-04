---
layout: notes
title: Release It! - Chapitre 4 - Suite et fin
date: 2025-06-04 00:00:00 +0100
categories: notes_de_lecture
---
Je continue la lecture du livre [Release It!](https://pragprog.com/titles/mnee2/release-it-second-edition/) de Michael Nygard. 
Voici mes notes sur le chapitre 4, _Stability Antipatterns_. 
Ce chapitre étant long, je le lis en plusieurs fois. 
Aujourd'hui la fin de ce chapitre. 

**Blocked Threads**.  
Les threads bloqués menacent la stabilité d'une application. 
Cela arrive souvent avec les pools de ressources, comme les pools de connexions à la base de données. 
On évite de recréer soi-même du code pour gérer les pools de connexions. 
On utilise des libs existantes qui ont fait leur preuves.  
Un timeout doit toujours être configuré quand on acquiert une ressource. 

**Self Denial Attacks**.  
Il peut arriver aussi que l'on mette en danger son propre système sans le vouloir. 
Par exemple une campagne commerciale qui va s'emballer et générer un énorme trafic qui fera tomber les serveurs. 

> Send mass emails in waves to spread out the peak load. 
> Create static “landing zone” pages for the first click from these offers. 
> Watch out for embedded session IDs in URLs. 

**Slow Responses**.  
> generating a slow response is worse than refusing a connection or returning an error, particularly in the context of middle-layer services. 

Une réponse lente peut entraîner des échecs en cascade dans un système. 
Cela peut aussi augmenter trafic, les utilisateurs impatients rechargeant les pages. 
Il vaut mieux échouer rapidement si on dépasse un certain seuil. 

**Unbounded Result Sets**.  
Il faut aussi faire attention à la taille de données que l'on manipule. 
Au départ on peut avoir un volume réduit, mais celui-ci peut croître de façon innattendue. 
Il faut limiter la taille des résultats. 

En dev, on utilise des volumes de données réalistes. 

> In any API or protocol, the caller should always indicate how much of a response it’s prepared to accept.