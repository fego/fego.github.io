---
layout: post
title: Communication Patterns - Notes de lecture - Chapitre 11
date: 2025-04-01 00:00:00 +0100
categories: notes_de_lecture
---
### Feedback
Il faut prendre du feedback tôt et souvent. 

> One mistake that many people make is putting a lot of time and effort into their work before they get any feedback on it. 
> This can waste effort and money as well as impact the architectural design of a system. 
> This applies both to individuals and teams.

Plus on travaille en tunnel sur un document, plus on est exposé au biais des coûts irrécupérables

> the longer you work on something, the less you want to make changes to it

Les avantages de récupérer du feedback fréquemment et tôt : 
- On peut identifier les erreurs, problèmes et risques au plus tôt, et donc réagir vite
- On peut voir au plus tôt les axes d’amélioration
- On s’assure d’être aligné avec les besoins
- On établit un dialogue 

Ce peut être intéressant de demander aussi un retour de la part de personnes non impliquées directement. 

Un cas particulièrement important  pour demander du feedback est lorsque l‘on formule les hypothèses de travail sur un document. 
Si elles sont fausses ou incomplètes on pourrait alors travailler inutilement. 

Si on est nouveau dans un rôle ou poste, ne pas hésiter à solliciter ses collègues : 

> Consider any frustration at your requests for input to be short-term, whereas the cost to your reputation of producing something that is ultimately “wrong” or costly will be more lasting.

Comment demander du feedback ? 
- On peut montrer simplement son travail, sans faire de demande explicite
- On peut mettre en place des processus formels, comme les *pull requests* ou des ADRs.

### Partager la charge de la documentation
Il est possible d'affecter des rôles spécifiques à des personnes de l'équipe sur la documentation. 
Mais jamais une seule personne sur un rôle, au risque sinon d’être moins pertinente et obsolète. 
Et cela pourrait créer des goulots d'étrangelement, ou pire si la personne est absente ou démissionne (bus factor). 

> Ensure you do not have a single point of failure (one person responsible for something) when it comes to documentation and communication

Utiliser des formats non propriétaires, afin d’augmenter le nombre de personnes pouvant participer à la documentation. 

Penser aussi à utiliser des notations accessibles aux personnes qui devront lire (et / ou modifier). 
Un diagramme UML ou ArchiMate réduit le nombre de personnes capables de le consulter. 
D’autres standard sont plus simples : [C4](https://c4model.com/), diagrammes de flux, des diagrammes des séquence simplifiés par exemple. 

> A standard such as UML or ArchiMate may end up reducing access. 
> The number of people with a full understanding or qualification in these standards is small

Privilégier les outils collaboratifs, en temps réel ou asynchrone (Google docs, Miro, etc) pour favoriser l’échange de connaissance. 

### Just-in-Time Architecture
Il faut différer les décisions d’architecture au plus tard possible (note : un des intérêts notamment de l'architecture haxagonale). 
Ces décisions sont très structurantes et leur réversibilité a un coût élevé. 
En les prenant au bon moment on travaille uniquement avec les dernières informations disponibles, évitant de travailler inutilement. 
On a plus d’opportunités pour acquérir des connaissances importantes, on réduit les risques, on a plus de temps pour échanger, dialoguer. 
Cela réduit la complexité. 
Donc on ne documente que ce dont on a besoin maintenant. 

Cependant, dans certains cas on peut noter des choses utiles pour le futur. 
On garde une place à part pour cela dans la documentation. 
Mais on ne mélange pas ces informations avec le présent. 

Que faire si on nous presse de fournir de la documentation plus tôt que ce que l’on juge pertinent ? 
Fournir un travail en cours clairement indiqué comme brouillon. 
Montrer l’intérêt de travailler au bon moment. 

Si on nous demande des estimations, des dates ? 
Rappeler que les équipes qui estiment le moins sont celles qui produisent le plus. 

> Jeff Sutherland, inventor and cocreator of Scrum, [says](https://www.quora.com/What-are-the-techniques-set-by-the-Scrum-guidelines-for-a-task-estimation-in-sprint-planning-Are-there-any-limitations-to-these-techniques) “estimating tasks will slow you down. Don’t do it. We gave it up over 10 years ago. Today we have good data from Rally on 60,000 teams. The slowest estimate tasks in hours. No estimation at all will improve team performance over hour estimation.”

De la documentation obsolète ou fausse est très dommageable : 

> When documentation is incorrect, or its maintenance costs much more than it delivers, the net overall value of the documentation will be negative.

Donc il faut la supprimer ou l'archiver. 

> You can combine *just-in-time architecture* with *just-long-enough* architecture: retire documentation and artifacts that have served their purpose and make it obvious that they’re no longer up-to-date. Just-long-enough architecture saves time you would have spent maintaining content that is no longer useful and prevents readers from consuming out-of-date information.

[Notes du prochain chapitre]({% link _posts/2025-04-09-Communication Patterns - Chapitre 12.md %})