---
layout: post
title: Communication Patterns - Notes de lecture - Chapitre 1
date: 2025-03-20 12:00:00 +0100
categories: notes_de_lecture
---
Je vous partage mes notes sur ma lecture du livre [Commmunication Patterns](https://communicationpatternsbook.com/) de Jacqui Read. 

La première partie du livre parle de communication visuelle. 
Elle traite principalement des diagrammes.  
Aujourd'hui, le chapitre 1, qui parle des éléments de base pour réussir un bon diagramme, et les pièges à éviter. 

## Pattern : Connaître son audience
Il faut savoir qui va consulter le diagramme que l'on crée : quels rôles (dev, architecte, po, etc). 
En fonction de groupes de rôles on pourrait être amené à faire différents diagrammes (avec différents niveaux d’abstraction) :  

> Make a list of the roles that view your diagrams and then group the roles based on the types of diagrams you create. You will likely find that you have a different audience mix for different diagrams.

Répondre aux questions par groupes de rôles identifiés : 
- ce qu’ils veulent savoir
- ce que l’on veut d’eux (qu’ils s’informent uniquement ? une validation ? une décision ?)
- quel est leur niveau technique (et ce que cela leur permet de comprendre)
- quel niveau de détail leur est nécessaire

## Antipattern : mélanger plusieurs niveaux d’abstraction
Comme pour le code, on évite d’avoir plusieurs niveau d’abstraction. 
Mélanger différents niveaux crée de la confusion. 
Donc **1 diagramme = 1 niveau d'abstraction**. 

Certains types de diagrammes nous obligent à avoir cette réflexion comme les [diagrammes C4](https://c4model.com/) qui proposent 4 niveaux d’abstraction. 
On va faire plusieurs diagrammes à différents niveaux d’abstraction si nécessaire. 

## Pattern : Cohérence représentationnelle
On est cohérent entre les différents diagrammes que l'on propose. 
Quand des diagrammes ont des composants en commun, on doit pouvoir facilement les identifier. 
Exemple : On a un diagramme qui montre un composant dans sa globalité, avec ses interactions avec d'autres composants et acteurs. 
Si on zoom sur l'intérieur d'un composant dans un autre diagramme, on doit pouvoir comprendre rapidement de quel composant il s'agit, et pouvoir naviguer facilement entre les diagrammes. 
Soit le type de diagramme propose ces connexions de façon formelle, soit il faut trouver un moyen de faire ces connexions soi-même. 

Il est important de mettre un libellé à ses diagrammes, et les référencer explicitement.