---
layout: post
title: Coursera - Prompt Engineering for ChatGPT - Module 1 à 3
date: 2025-05-06 00:00:00 +0100
categories: notes_de_lecture
---
J'ai commencé le mooc [Prompt Engineering for ChatGPT](https://www.coursera.org/learn/prompt-engineering) sur Coursera. 
Je vous partage mes notes. 
Aujourd'hui les modules 1 à 3. 
Les modules 1 et 2 sont des modules introductifs que j'ai trouvés un peu longs. 
Le module 3 attaque le vif du sujet : le catalogue de patterns. 

## Intro

Un LLM essait de générer la suite (le mot / token suivant) de notre prompt. 
Si on commence une phrase, il va la terminer. 

Par exemple si on fournit le prompt “*Roses are red*”, la réponse de Gemini est un poème classique : 

> Violets are blue,
> Sugar is sweet,
> And so are you.
 
Mais si on relance le même prompt, on pourrait avoir une autre sortie. 
Il y a un côté aléatoire. 

Les LLMs évoluent très vite.

Prendre en compte que les modèles sont entraînés à un instant T. 
Et n’ont donc pas les connaissances qui sont après cette date. 
Il faudra leur fournir les informations nécessaires si celles-ci sont après la date de fin d’entraînement (*cutoff*), ou que le modèle puisse aller les chercher sur internet. 

Si on veut des réponses spécifiques, il faut une question spécifique.

Il y a une taille limite au prompt. 
Il faut donc mettre les informations les plus importantes. 
On fait un filtre, on compresse. 
On peut utiliser un LLM pour faire cette compression d'ailleurs. 

Souvent il faut aborder un LLM sous forme de conversation, plus qu’un seul prompt pour avoir tout de suite la réponse. 

## Catalogue de patterns

### Persona Pattern
On donne un rôle au LLM. 
Cela influence le contenu et la forme. 

Structure type : 
- Act as Persona X
- Perform task Y

Ex : 
> Act as a skeptic that is well-versed in computer science. 
> Whatever I tell you, provide a skeptical and detailed response. 

En français : 
> Agis comme un sceptique bien informé en informatique. 
> Quelle que soit l'information que je te donne, réponds de manière sceptique et détaillée.* 

Voici la version proposée par ChatGPT si je lui demande d’être un expert en prompt engineering : 
> Adopt the role of a critical thinker with deep expertise in computer science. 
> For every statement I provide, respond with a detailed and skeptical analysis, questioning assumptions, highlighting potential flaws, and offering technical counterpoints where relevant.

### Question Refinement Pattern

Faire en sorte que le LLM améliore notre question. 

Prompt : 

> Whenever I ask a question, ask me additional questions to clarify what I'm asking before you provide an answer.

ou
> Whenever I ask a question, suggest a better question and ask me if I would like to use it instead.

Voici une version modifiée par ChatGPT : 
> Whenever I ask a question, ask me additional questions to clarify what I'm asking only if my question is ambiguous or could have multiple interpretations. 
> If it's clear, go ahead and answer directly.

Prompt FR : 

> Chaque fois que je pose une question, pose-moi d'autres questions pour clarifier ce que je demande avant de donner une réponse.

ou 
> Chaque fois que je pose une question, suggère une meilleure question et demande-moi si je souhaite l'utiliser à la place.

Et la version modifiée : 
> Chaque fois que je pose une question, pose-moi des questions supplémentaires pour clarifier ce que je demande, uniquement si ma question est ambiguë ou peut donner lieu à plusieurs interprétations. 
> Si elle est claire, réponds directement.

### Cognitive Vérifier Pattern
Prompt : 
> When you are asked a question, follow these rules.
> Generate a number of additional questions that would help you more accurately answer the question. 
> Combine the answers to the individual questions to produce the final answer to the overall question.

Prompt FR : 
> Lorsque je te pose une question, suis ces règles. 
> Génère un certain nombre de questions supplémentaires qui t’aideront à répondre plus précisément à la question. 
> Combine les réponses aux différentes questions pour obtenir la réponse finale à la question globale.

Ce prompt se base sur le fait que les LLMs raisonnent mieux si on divise la question en une série de questions qui vont aider à formuler une réponse. 
Ce n’est pas parce que les modèles actuels ont des capacité de raisonnement intégrées que ce pattern n’est plus utile. 
Je pense qu’il peut aider à converger plus vite vers une solution pertinente. 

Exemple d’une question que j'ai posée à Gemini, sachant qu’il est “préconfiguré” (un “Gem” de Gemini, équivalent des “GPTs” de ChatGPT) avec un prompt qui lui demande de faire cette vérification. 

Prompt : *Je veux faire un test avec junit, sans charger le contexte de mon application, mais uniquement les clients feign correctement configurés. Comment faire ?* 

Réponse : 

> Pour t'aider à cibler au mieux la configuration de tes tests d'intégration Feign sans charger tout le contexte Spring, j'aurais besoin de quelques précisions :
> 
> 1. **Comment tes clients Feign sont-ils configurés actuellement ?** (Utilises-tu `@FeignClient` avec une URL statique, un `configuration` spécifique, ou un `serviceId` pour la découverte de services ?)
> 2. **As-tu des configurations Spring spécifiques liées à tes clients Feign (interceptors, encoders, decoders, error decoders) ?**
> 3. **Souhaites-tu mocker les services externes appelés par tes clients Feign, ou interagir avec de vrais services de test ?**
> 4. **Quel est le but précis de ce test unitaire ciblé ?** (Vérifier la sérialisation/désérialisation, les headers, les erreurs, etc. ?)
> 
> Une fois ces éléments clarifiés, je pourrai te guider vers une solution plus précise.

Sans ce pattern, la réponse est beaucoup plus directe : 

> Ta question est claire. Pour tester tes clients Feign sans charger tout le contexte Spring, tu peux utiliser @SpringBootTest avec une configuration minimale ou mock tes clients Feign.
> 
> Quelle approche souhaites-tu explorer en premier ? Veux-tu une solution basée sur une configuration spécifique ou sur le mocking ?

### Audience Person Pattern
On demande au LLM de produire une réponse pour une audience particulière. 
On peut lui indiquer notre niveau de connaissance, expertise sur le sujet. 

Modèle :

- Explain X to me.
- Assume that I am Persona Y.

### Flipped Interaction Pattern
Quand on ne sait pas exactement où l'on va, quelle est la bonne question à poser, ce qui est important comme contexte, on va affiner cela avec le LLM.

C'est lui qui va nous poser des questions pour arriver à nos fins.
Structure type :
- I would like you to ask me questions to achieve X
- You should ask questions until condition Y is met or to achieve this goal (alternatively, forever)
- (Optional) ask me the questions one at a time, two at a time, ask me the first question, etc.
  
Exemple : 
> I would like you to ask me questions to help me create variations of my marketing materials. 
> You should ask questions until you have sufficient information about my current draft messages, audience, and goals. 
> Ask me the first question.

En français: 
> J'aimerais que tu me poses des questions pour m'aider à créer des variantes de mes supports marketing. 
> Tu dois poser des questions jusqu'à ce que tu aies suffisamment d'informations sur mes messages actuels, mon public et mes objectifs. 
> Pose-moi la première question.

[Notes du prochain module]({% link _posts/2025-05-07-Coursera-Prompt Engineering For ChatGPT.md %})