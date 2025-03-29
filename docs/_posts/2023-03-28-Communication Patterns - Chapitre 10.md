---
layout: post
title: Communication Patterns - Notes de lecture - Chapitre 10
date: 2025-03-28 00:00:00 +0100
categories: notes_de_lecture
---
Aujourd'hui, je commence la 3ème partie du livre [Communication Patterns](https://communicationpatternsbook.com/) de Jacqui Read. 
Une partie sur la communication de la connaissance. 
Dans le chapitre 10 l'autrice aborde la mise en place d'une documentation produit. 

## Products over projects
Il faut organiser sa documentation par produit et non projet. 
Les projets sont transitoires, pour du court terme. 
La documentation spécifique à un projet peut être pertinente, mais doit se retrouver dans la documentation d’un produit. 

On organise la documentation par produits car : 
* Ils sont à long terme, et donc pérennes.  
* Il est plus facile de retrouver de la documentation et de la réutiliser en l’organisant par produit. 
* Cela favorise aussi la collaboration. 
* On donne plus facilement l’attention au client 
* Et cela favorise une démarche d’amélioration continue. 

Si on a plusieurs produits, on a un seul outil qui regroupe toutes les documentations. 

On privilégie les tags et métadonnées aux dossiers. 
Si on utilise des dossiers, on évite les hiérarchies compliquées qui figent les catégories. 
Les documents peuvent souvent se retrouver dans plusieurs catégories, ce que permettent les tags. 
Si l’outil le permet on peut utiliser d’autres métadonnées pour classifier les documents. 
Cela peut permettre de limiter les hiérarchies de dossier. 

> Folders are an outdated system because they force an artifact to belong in only one place. An artifact can have multiple categories and therefore can have multiple tags 

> If you are using folders, or don’t have the option to use tags, organize by product at the highest level possible to gain the most holistic view of your products

## Abstractions over text
Il est souvent utile de communiquer autrement qu’avec du texte pur. 
En dehors des diagrammes et des graphiques (et dans une moindre mesure des images, des vidéos, des audios), l'autrice présente quelque moyens d'améliorer la communication : 

### Les listes
Utiles pour synthétiser des informations. 
Les bonnes pratiques pour faire une liste : 
- mettre ce qui est le plus important en premier
- avoir un style cohérent dans chaque élément :
    - commencer toujours de la même façon (verbe, nom, …).
    - avoir une longueur identique et plutôt courte
- mettre de l’emphase (gras, italique) sur la première phrase si on dépasse en longueur
- limiter l’usage des sous listes

### Les tableaux
Utiles notamment pour : 
- exposer de l’information répétitive
- pour comparer des données
- avoir plus de précision que sur un graphique

Par contre on prend moins vite connaissance de l’information que sur un graphique. 

Tips pour créer un tableau : 
- Introduire le tableau par une phrase.
- Ne pas surcharger les cellules
- Un en-tête clair, en gras et / ou plus grand
- Un seul type de donnée par colonne

### Harvey Balls
[Voir sur wikipedia](https://en.wikipedia.org/wiki/Harvey_balls). 
Un moyen visuel pratique pour montrer par exemple à quel point un élément satisfait un critère. 

## Perspective-driven documentation
Les parties prenantes (dev, archi, po, sécu, clients, etc) ont des besoins différents en terme de connaissance. 
Et souvent on fait des documents qui tentent de les satisfaire tous. 
Il est donc assez difficile d'accéder à l'information dont on a besoin, car elle peut être noyée dans d'autres informations qui ne nous intéressent pas. 

Une perspective permet d'adresser ces différences de besoins. 
Elle s'adresse à une partie prenante et à ses besoins. 
Une perspective est soit atomique, et alors elle contient un élément de base, comme un texte, un diagramme, un graphique, etc. 
Soit elle est composée d'autres perspectives, et est donc un assemblage de différents éléments (Jacqui Read parle de perspective fractale). 

> a *perspective* is a collection of one or more artifacts that address one or more (typically related) concerns of a particular stakeholder

Puisqu'une perspective peut être incluse dans une autre, elle doit être réutilisable. 
Il ne faut jamais répéter une information qui est déjà présente (_Don’t Repeat Yourself_), car cela devient trop compliqué à maintenir. 
Donc on inclut les éléments existants dont on a besoin :

> The tools and applications must allow you to embed an artifact in more than one perspective and for that artifact to automatically update everywhere when the original is updated 

Si on ne peut pas inclure directement un élément, on peut faire un lien, si celui-ci est dynamique (= par exemple si on change le nom de la page, le lien est automatiquement mis à jour par l'outil partout où il est référencé). 

Dans certains (rares) cas on fera une copie, si on veut une version précise d’une information. 

Il est intéressant d’extraire des modèles quand on répète un formalisme, afin de le standardiser. 

TIPs sur les diagrammes : utiliser des calques pour pouvoir réutiliser des parties dans différentes situation (visiblement [draw.io](http://draw.io) le permet). 

Note : le terme “vue” aurait pu être utilisé à la place de perspective, mais l’autrice trouve que ce terme est trop connoté dans le dev et l’archi. 