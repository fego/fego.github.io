---
layout: notes
title: Notes sur l'article d'Arthur Perret sur l'usage des LLMs dans un but informationnel
date: 2025-06-21 00:00:00 +0100
categories: notes_de_lecture
---
Aujourd'hui je vous partage quelques notes sur l'article [L’intelligence artificielle générative dans l’impasse informationnelle](https://www.arthurperret.fr/articles/2025-06-20-congres-sfsic-ia-impasse-informationnelle.html) d'Arthur Perret, dont je vous recommande la lecture. 
Il m'a donné pas mal de matière à réflexion. 

Pour l'auteur, les LLMs sont dans une impasse concernant leur usage à but informationnel. 
Ce sont avant tout "des machines à communiquer, pas à informer". 

Il adresse des risques globaux dans l'usage des LLMs.  
Un danger qu'il remonte dans leur usage m'a particulièrement marqué. 
Alors qu'un LLM n'est qu'un outil qui génère du texte, on y trouve tout de même du sens, des idées, croyances et autres. 

> lorsque nous utilisons un programme basé sur un LLM, nous sommes prédisposés à détecter des éléments non-linguistiques (croyances, intentions, contexte) qui sont en réalité absents du fonctionnement des LLM.

Dans les recherches qui sont faites sur ces outils, il ressort qu'ils peuvent entraîner une perte de nos capacités de mémorisation et d'esprit critique. 

> Les risques cognitifs, par exemple, commencent à être documentés : les gains de performance associés à l’utilisation d’un programme comme ChatGPT s’accompagnent d’une dépendance à la machine, avec une diminution des capacités de mémorisation, ou d’esprit critique. 

Le problème fondamental pour leur usage informationnel est ce qui est communément appelé "hallucinations". 
Les LLMs se trompent souvent. 
Alors pour pallier cela on a développé des techniques : 
* prompt engineering (pas cher à mettre en œuvre mais fragile)
* le fine-tuning (beaucoup plus cher et plus efficace)
* le rag (on augmente le contexte avec de l'information dans un premier temps)

Il y a assez peu de publications à ce jour sur ces différentes techniques. 
Mais quoi qu'il en soit, au final il y a toujours des erreurs. 
Autant, dans certains cas d'usage cela peut être tolérable, ici, ce ne l'est pas. 
Les moteurs de recherche basés sur des LLMs font beaucoup d'erreurs encore. 
Une fois sur deux ils ne citent pas leur source, et une fois sur quatre la source citée ne contient pas les éléments liés à l'information pour laquelle elle est citée. 
La fonctionnalité de _deep research_ est plus performante, mais plus coûteuse (et toujours imparfaite). 

Il cite Benedict Evans (avec sa traduction) : 

> Dire que les modèles s’améliorent sans cesse […] est hors sujet. Si le modèle d’aujourd’hui génère un tableau correct à 85% et que la prochaine version atteindra 85,5 ou 91%, cela ne m’aide pas. Peu importe combien d’erreurs il y a dans le tableau – s’il y en a, je ne peux pas m’y fier

Les LLMs utilisent le même procédé pour dire quelque chose de vrai ou de faux. 
Arthur Perret reprend le terme de _bullshit_, proposé par Hicks, Humphries et Sater. 
Ce terme parait plus juste car l'idée d'hallucination est trompeuse, faisant croire à une erreur accidentelle. 
Benoît Raphaël dit à ce sujet dans sa formation qu'ils hallucinent en permanence. 
Je me souviens aussi de [cette vidéo](https://www.youtube.com/watch?v=R2fjRbc9Sa0&pp=ygUVbW9uc2lldXIgcGhpIGJ1bGxzaGl0) de Monsieur Phi qui utilisait déjà ce terme. 

> Les « bullshitters » ne se préoccupent pas de savoir si ce qu’ils disent est vrai ou faux ; ce qui compte, ce sont les effets que leurs énoncés produisent

Ce qui est généré est vraisemblable, mais pas forcément vrai. 

Le caractère d'un média (son impact sur notre vie, comme par exemple la télé qui nous rend passifs, nous influence sur notre rythme de vie, etc.) nous influence plus que son contenu (McLuhan). 
Un LLM n'est pas un média d'information, mais de communication discurive. 
Il y a une illusion de contenu fiable (notamment à cause de la crédibilité du ton). 
Il pousse à des usages informationnels inadaptés. 

> Ce faisant, on n’examine pas la nature de ChatGPT en tant que média, son caractère ; or c’est bien ce dernier (nous dit McLuhan) qui influence le plus nos activités, et qui peut affecter durablement l’évolution de nos sociétés. Ce qui caractérise ChatGPT, nous l’avons vu, c’est que la relation entre les discours et les faits échappent à sa définition du sens. Peu importe qu’on réduise son taux d’erreurs : le problème est qu’il s’exprime sous un régime de vérité bien particulier, qui traite les faits sous l’angle communicationnel plutôt qu’informationnel.

Donc se servir des LLMs dans un cadre informationnel devrait à minima se faire en étant informé de tous ces risques. 

> L’ensemble des réflexions présentées dans cet article peuvent justifier le choix du non-usage des programmes basés sur des LLM pour répondre à des besoins d’information. Idéalement, ce choix devrait intervenir de manière libre et éclairé au terme d’actions relevant de l’éducation aux médias et à l’information (EMI), dans une perspective d’autonomisation des individus