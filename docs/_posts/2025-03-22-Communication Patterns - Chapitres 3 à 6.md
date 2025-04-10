---
layout: post
title: Communication Patterns - Notes de lecture - Chapitres 3 à 6
date: 2025-03-21 07:00:00 +0100
categories: notes_de_lecture
---
Voici mes notes de lecture des chapitrs 3 à 6 du livre Communication Patterns de Jacqui Read. 

## Accessibilité
Il faut considérer si toute l’audience d’un diagramme pourra le lire. 
Que ce soit pour une question de déficience, ou d’environnement. 

### Attention à l'usage de la couleur. 
Tout le monde ne perçoit pas les couleurs de la même façon. 
Pour aider la lecture d'un diagramme avec des couleurs, on peut compléter les couleurs par des signes, comme par exemple sur un diff qui a les signes + et -. 
On peut aussi mettre des motifs, notamment si on a des nuances de gris au final. 

On peut s’aider d’outils pour voir ce que cela donnerait, par exemple :
- [Color Oracle](https://colororacle.org/) une app qui permet de simuler différentes déficiences
- [Coblis](https://www.color-blindness.com/coblis-color-blindness-simulator/) le fait en ligne

### Usage d'une légende
Une légende est souvent intéressante à ajouter. 
Elle doit clarifier le diagramme. 
Si on n’en met pas, on doit avoir une bonne raison. 

## Notation

### Antipattern : Utiliser des icônes pour transmettre de l'information
On utilise des icônes comme **supplément** d’information, mais pas comme unique moyen d’informer (sauf exceptions, quand une icône est très répandue). 
Si on ne connaît pas l’icône, on ne comprend pas le message. 
Donc une icône **doit** avoir une étiquette. 

Challenger l’icône : apporte-t-elle une vraie plus-value ?

### Antipattern : Utiliser UML pour utiliser UML
UML a une notation qui peut être complexe, même pour l’auteur du diagramme. 
Challenger son usage avec des diagrammes au formalisme plus simple, comme C4. 

S’autoriser à simplifier la notation UML. 

### Antipattern : Mélanger du comportement et de la structure
Mélanger du comportement et de la structure apporte de la confusion. 
Un diagramme a une une responsabilité, une raison d'être. 
Un diagramme de structure informe sur le *quoi* et *où*. 
Un diagramme de comportement informe sur le *comment* et *vers qui*. 

## Composition

### Antipattern : Composition trompeuse
Il faut faire attention à garder des échelles cohérentes entre les éléments. 
Si un élément est plus gros ou plus petit dans un diagramme, on peut interpréter cette différence de taille. 
Et donc il faut que cette différence soit justifiée. 
Si ce n'est pas le cas, alors on doit avoir la même taille. 

Voici pour les notes sur la première partie concernant la communication visuelle, et donc principalement les diagrammes. 
Le livre enchaîne sur la communication multimodale, avec un premier chapitre sur la communication écrite. 

[Notes du prochain chapitre]({% link _posts/2025-03-23-Communication Patterns - Chapitre 7.md %})