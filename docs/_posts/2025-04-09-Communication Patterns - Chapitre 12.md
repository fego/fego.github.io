---
layout: notes
title: Communication Patterns - Notes de lecture - Chapitre 12
date: 2025-04-09 00:00:00 +0100
categories: notes_de_lecture
---
Je continue mon partage de notes du livre [Communication Patterns](https://communicationpatternsbook.com/) de Jacqui Read. 
Aujourd'hui le chapitre 12. 
Au menu de ce chapitre : les ADRs, les caractéristiques d'une architecture et _documentation as code_. 
Dans mes notes je commence par la partie caractéristiques, car elle peut servir dans les ADRs. 
Je ne partage rien sur le _doc as code_ car je n'ai pas été emballé par cette partie (bien que le sujet soit très intéressant). 

### Caractéristiques d’architecture
Ce sont les priorités du système ou produit, les exigences non fonctionnels. 
La liste de toutes les caractéristiques peut être très longues. 
Parmi toutes celles existantes, on doit en choisir 7 max. 
Et désigner le top 3. 
Cependant certaines peuvent être considérées comme implicites et ne sont donc pas comptées dans les 7. 
Cela concerne : la faisabilité (coût/temps), la maintenabilité, la sécurité, la simplicité (note : Mark Richards rajoute sur son modèle l'observabilité). 
Voici des listes de carctéristiques qui peuvent être utiles : 
- [la liste de Wikipedia](https://en.wikipedia.org/wiki/List_of_system_quality_attributes)
- [la liste iso 25000](https://iso25000.com/index.php/en/iso-25000-standards/iso-25010)
- [la liste de Mark Richards](https://web.archive.org/web/20250121221235/https://developertoarchitect.com/resources.html)

La liste des caractéristiques retenues va évoluer au cours de la vie du système. 

### ADR
Une ADR contient une décision d'architecture, mais peut aussi contenir le raisonnement ayant conduit à cette décision. 
Ce n’était pas le cas à l’origine [quand Michael Nygard a introduit ce concept](https://web.archive.org/web/20250402141237/https://cognitect.com/blog/2011/11/15/documenting-architecture-decisions). 

Pourquoi utiliser une ADR ?
- on comprendra mieux dans le futur le pourquoi d’une décision, on peut avoir oublié le contexte
- on évite les décisions qui reviennent en boucle
- elles peuvent aider l’onboarding
- elles évitent de la perte de connaissance quand quelqu’un part

Voici la structure d’une ADR qu'elle propose : 
- Titre et nom du fichier : commencer par un identifiant et la décision (ex : 001 Use event-driven architecture)
- Statut : Brouillon, Décidée ou Remplacée. C’est le seul élément modifiable d’une ADR après sa validation (Décidée → Remplacée par une autre ADR).

> I recommend using *Decided* instead of *Accepted* and *Rejected* statuses for ADRs. 
> The decision is always made and, most of the time, it is about selecting one of multiple options rather than a yes/no decision.
 
- Contexte : expliquer pourquoi on doit prendre cette décision. 
On indique les hypothèses, les contraintes, et tout ce qui pourrait avoir de l’importance dans la décision.
- Critères d’évaluation : On liste ici les critères d’évaluations. 
Par exemple les caractéristiques d’architecture que l’on a sélectionnées.
- Options : La liste des possibilités et leur évaluation (harvey balls, ou autre). 
Etre honnête et transparent, donc mettre les compromis.
Évaluer juste le nombre d’options nécessaires. 
- Décision : Une simple phrase.

> This doesn’t need to be long and complicated because most details about the decision are included under the other headings.
 
- Implications : Les conséquences, positives ou négatives, de la décision.
- Consultation : Une sorte d’annexe ou l’on peut indiquer des conseils, des ressources.

Exemples sur son site : https://communicationpatternsbook.com/bonuses.html

L’accès aux ADRs doit être facile pour toutes les personnes devant les consulter. 

Pour favoriser la mise en place d’ADR, il faut montrer l’exemple en en faisant soi même, et en rendant le processus de création simple. 

Le processus de décision et la décision finale sont de la responsabilité de la personne avec le plus d’expertise sur le sujet, et donc pas forcément un senior. 

Les parties prenantes doivent s’engager à respecter la décision. 
S’ils ne s’engagent pas, il faut alors creuser avec eux les blocages. 
On peut s’engager sans être d’accord avec la décision.