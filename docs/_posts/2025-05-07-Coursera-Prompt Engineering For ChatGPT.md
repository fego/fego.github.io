---
layout: post
title: Coursera - Prompt Engineering for ChatGPT - Module 4
date: 2025-05-07 00:00:00 +0100
categories: notes_de_lecture
---
Je continue le mooc [Prompt Engineering for ChatGPT](https://www.coursera.org/learn/prompt-engineering) sur Coursera. 
Je vous partage mes notes. 
Aujourd'hui le module 4. 
Un découvre une famille de prompt qui se basent sur le même principe. 
On montre ce que l'on souhaite, cela va influencer le contenu et la structure du résultat. 

### Few-Shot Examples
On montre des exemples à suivre par le LLM. On n'a pas besoin d'expliquer ce que l'on veut, on peut se contenter de montrer ce que l’on attend comme type de réponse.

Structure type :

> Input : \<*Input d’exemple*>  
> Output : \<*Output d’exemple*>  
> Input : \<*Input d’exemple*>  
> Output : \<*Output d’exemple*>  
> (répéter autant de fois que nécessaire pour être clair) 
> Input : \<*input dont on cherche la réponse*>  
> Output : \<vide>  

Exemple pour une analyse de sentiment :

> Input : I didn’t really like this book, it lacked important details and didn’t end up making sense  
> Sentiment : Negative  
> Input : I loved this book, it was really helpful in learning how to improve my gut health  
> Sentiment : Positive  
> Input : I wasn’t sure what to think of this new restaurant, the service was slow, but the dishes were pretty good  
> Sentiment :  

Réponse du LLM : *Neutral*

On peut utiliser cette technique pour obtenir des actions. 
Par exemple :

> Situation : I'm traveling 60 miles per hour and I see the brake lights on the car in front of me come on.  
> Action : Brake  
> Situation : I've just entered the highway from an on-ramp and I'm traveling 30 miles per hour.  
> Action: Accelerate  
> Situation : a deer has darted out in front of my car while I'm traveling 15 miles per hour and the road has a large shoulder.  
> Action: Brake and swerve into shoulder  
> Situation: I'm backing out of a parking spot and I see the reverse lights illuminate on the car behind me.  
> Action : \<vide>

Réponse du LLM :

> Stop and wait for the car to back out before proceeding

On peut demander au LLM de générer d’autres exemples aussi si besoin d'être plus exhaustif. 

On peut aussi donner des étapes intermédiaires.
Par exemple on peut alterner des étapes "Situation" / "Think" / "Action". 
On se rapproche alors d'un pattern de type _Chain of Thought_. 

### Chain of Thought Prompting
Chain of Thought Prompting se base sur le Few-Shot prompting. 

Le but est d'inciter le LLM à expliquer son raisonnement. 
Il a plus de chance de produire une bonne réponse.

Structure type :

> Question : \<exemple de question>  
> Answer :  
> * Reasoning : \<exemple de raisonnement pour répondre à la question>  
> * Answer : \<réponse>  
>
> (on répète d’autres exemples)  
>
> Question : \<question dont on veut la réponse et le raisonnement>  
> Answer :  
> * Reasoning : \<REASONING>  
> * Answer : \<ANSWER>  

Pour approfondir : [lien vers une étude](https://arxiv.org/abs/2201.11903)

### ReAct Prompting
Reason + Act = ReAct. 

Une technique pour apprend à un LLM à faire différentes actions, utiliser des outils externes pour résoudre des problématiques. Cette technique est une variante du Chain of Thought Prompting. 

Structure type :

> Task : \<exemple de tâche>  
> Think : \<exemple de raisonnement>  
> Action : \<exemple d’action>  
> Result : \<exemple de résultat>  
> (on répète les exemples)  
> Task : \<tâche dont on souhaite avoir un résultat avec le raisonnement et les actions>

En savoir plus : [lien vers une étude](https://arxiv.org/abs/2210.03629)