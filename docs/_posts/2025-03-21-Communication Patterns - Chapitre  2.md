---
layout: post
title: Communication Patterns - Notes de lecture - Chapitre 2
date: 2025-03-21 07:00:00 +0100
categories: notes_de_lecture
---
Voici la suite de mes notes sur le livre Communication Patterns de Jacqui Read. 
Aujourd'hui le chapitre 2. 
L'autrice continue les conseils sur la création de diagrammes. 

Précisions de vocabulaire : 
* Étiquette : label en anglais, texte qui permet de définir un élément
* Note : un numéro qui référence un composant et qui est détaillé dans un bloc séparé ou en bas du diagramme
* Annotation : un bloc de texte qui détaille un élément.

## Antipattern : Surcharge de couleur
Il faut donner un sens aux couleurs que l'on utilise, et limiter leur nombre. 
Utiliser des couleurs sans leur donner un sens est une perte d’énergie pour le lecteur. 
Et utiliser trop de couleurs, même si elles ont un sens, peut nécessité trop d’effort cognitifs pour les comprendre.  

On ne met pas que des couleurs vives. 
On utilise aussi des couleurs douces. 
Et on met une légende pour expliquer le sens des couleurs. 

## Antipattern : Des boîtes dans des boîtes dans des boîtes
Il faut éviter de surcharger un diagramme avec trop de boites dans des boites. 
Mettre une étiquette ou une note sur un composant est une bonne alternative à une boîte. 
Aussi, on peut envisager de séparer un diagramme en plusieurs diagrammes. 

On met éventuellement un fond (doux) pour la boîte. 
Et on peut aussi varier la bordure, avec des pointillés. 

*TIP* : La gestion des espaces est très importante dans un diagramme.

## Antipattern : Une toile d'araignée de relations
Il faut éviter autant que possible que des relations se croisent dans un diagramme. 
Si c’est inévitable, alors bien marquer l’intersection par un arc. 

*TIP* : Standardiser la façon de mettre les étiquettes dans un diagramme, rester cohérent. 

## Pattern : Equilibrer le texte
Dans un premier temps, alléger autant que possible le diagramme, retirer le texte superflu qui peut être deviné par le contexte. 
Une information indispensable mais qui alourdit un diagramme doit probablement être déplacée sur le côté (une annotation, ou des notes). 
Considérer aussi la possibilité de faire un autre diagramme.