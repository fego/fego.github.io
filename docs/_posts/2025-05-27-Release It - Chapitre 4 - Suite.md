---
layout: notes
title: Release It! - Chapitre 4 - Suite
date: 2025-05-27 00:00:00 +0100
categories: notes_de_lecture
---
Je continue la lecture du livre [Release It!](https://pragprog.com/titles/mnee2/release-it-second-edition/) de Michael Nygard. 
Voici mes notes sur le chapitre 4, _Stability Antipatterns_. 
Ce chapitre étant long, je découpe ma lecture par sous parties. 
Aujourd'hui les sections _Chain Reactions_, _Cascading Failures_, _Users_. 

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
