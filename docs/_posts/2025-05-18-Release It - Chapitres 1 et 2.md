---
layout: notes
title: Release It! - Chapitres 1 et 2
date: 2025-05-18 00:00:00 +0100
categories: notes_de_lecture
---
Je démarre le livre [Release It!](https://pragprog.com/titles/mnee2/release-it-second-edition/) de Michael Nygard. 
Voici mes notes sur les deux premièrs chapitres. 

## Chapitre 1 - Living in Production
C'est l'introduction du livre. 

### Regarder plus loin que juste un développement
Souvent on fait des choix de développement de court terme pour juste répondre à une feature plus rapidement, mais en mettant en risque la stabilité et la maintenabilité du système. 
Cela n’a de sens que si on regarde cela du point de vue limité à une équipe, pour répondre à un besoin à un instant T, limité par un budget. 
Dans le contexte d’une organisation cela n’a pas de sens, c’est un choix catastrophique. 
Un système passe beaucoup plus de temps de sa vie en condition opérationnelle et en maintenance, qu’en développement. 
C’est un non sens d’éviter un coût unique lors d’un développement. 
On le paiera avec un coût opérationnel récurrent qui au final sera beaucoup plus élevé. 

> Avoiding a one-time developmental cost and instead incurring a recurring operational cost makes no sense.

Il ne faut pas chercher à juste passer la QA, mais faire un vrai système robuste et pérenne. 

> Too often, project teams aim to pass the quality assurance (QA) department’s tests instead of aiming for life in production.

> But testing—even agile, pragmatic, automated testing—is not enough to prove that software is ready for the real world. 

### L'impact des décisions d'architecture

Les décisions du début sont celles qui auront le plus d’impact sur la forme finale d’un système. 
Elles seront difficilement réversibles. 
Or ce sont les décisions que l’on prend avec le moins d’informations. 
Il faut apprendre le plus tôt sur les conséquences de nos décision, via de petites itérations.

> The earliest decisions you make can be the hardest ones to reverse later. 

> It’s a terrible irony that these very early decisions are also the least informed. 

> I advocate any approach that begins the learning process as soon as possible

## Chapitre 2 - Case study : The Exception That Grounded an Airline

Il décrit un incident qu’il a vécu dans une compagnie aérienne. 
Un bug de prod qui a mis 3 heures a être résolu, mais dont les impacts ont mis encore plus de temps à se résoudre dans la réalité. 
L’incident a eu lieu sur un composant, qui a fait tomber en cascade les autres composants. 

Au final, ils se sont rendus compte que le code fautif était une simple exception non gérée autour de la fermeture d’un `Statement`. 
Lors d’une bascule de cluster de base de données, cela a entraîné la non fermeture des connexions à la base. 

```java
public class FlightSearch implements SessionBean {

    private MonitoredDataSource connectionPool;

    public List lookupByCity(. . .) throws SQLException, RemoteException {
    Connection conn = null;
    Statement stmt = null;

    try {
        conn = connectionPool.getConnection();
        stmt = conn.createStatement();

        // Do the lookup logic
        // return a list of results
        
    } finally {
        if (stmt != null) {
            stmt.close(); // ici pas de gestion de l'exception -> bug
        }

        if (conn != null) {
            conn.close();
        }
    }
    }
}
```

La leçon : il n’est pas acceptable qu’un composant face tomber tout le SI. 

> The worst problem here is that the bug in one system could propagate to all the other affected systems.

Des bugs sont inévitables, malgré toutes les revues de code que l’on peut faire, tous les tests que l’on peut mettre en place.

> Ultimately, it’s just fantasy to expect every single bug like this one to be driven out. 
> Bugs will happen. 
> They cannot be eliminated, so they must be survived instead.

A noter aussi que la priorité numéro 1 quand on est sur un incident c'est remettre en marche le système. 
On verra ensuite le pourquoi. 