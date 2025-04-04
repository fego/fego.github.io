---
layout: post
title: Faire un mini outil sur mesure avec un LLM
date: 2025-04-04 00:00:00 +0100
categories: article
---
Je vous partage un usage qui me paraît intéressant des LLMs. 
Cet usage fait écho au terme _vibe coding_ tel qu'utilisé à l'origine par [Andrej Karpathy](https://x.com/karpathy/status/1886192184808149383) : 
> There's a new kind of coding I call "vibe coding", where you fully give in to the vibes, embrace exponentials, and forget that the code even exists. It's possible because the LLMs (e.g. Cursor Composer w Sonnet) are getting too good. Also I just talk to Composer with SuperWhisper so I barely even touch the keyboard. I ask for the dumbest things like "decrease the padding on the sidebar by half" because I'm too lazy to find it. I "Accept All" always, I don't read the diffs anymore. When I get error messages I just copy paste them in with no comment, usually that fixes it. The code grows beyond my usual comprehension, I'd have to really read through it for a while. Sometimes the LLMs can't fix a bug so I just work around it or ask for random changes until it goes away. It's not too bad for throwaway weekend projects, but still quite amusing. I'm building a project or webapp, but it's not really coding - I just see stuff, say stuff, run stuff, and copy paste stuff, and it mostly works.

Ce terme a été aussi repris par [Simon Willison](https://simonwillison.net/2025/Mar/19/vibe-coding/). 
Ma compréhension : faire générer du code sans le vérifier, pour des projets "jouets" ou des POCs à jeter. 

Dans mon cas j'avais le besoin suivant. 
Je veux pouvoir avoir un timer qui alterne deux durées, une pour une phase de travail et une pour une phase de pause. 
Un [pomodoro](https://fr.wikipedia.org/wiki/Technique_Pomodoro). 
Il existe déjà des outils qui permettent cela. 
Mais je voulais une fonctionnalité qui n'existe pas sur les outils que j'ai trouvés. 
Je souhaite que le timer se mette en route au chargement de la page et ne s'arrête jamais. 
Qu'il soit toujours en train d'alterner. 
En effet, avec un pomodoro classique, j'ai tendance à ne pas le démarrer dès le matin, et parfois il m'arrive de passer une journée sans le mettre en route. 
Donc je voulais voir si le fait de déclencher automatiquement les phases pouvait m'inciter à mieux les respecter. 

J'ai donc voulu voir si je pouvais générer un tel outil avec un LLM, sachant que je n'avais pas le courage de le faire moi-même. 
Je suis parti d'un [prompt de Harper Reed](https://harper.blog/2025/02/16/my-llm-codegen-workflow-atm/) pour générer ma specification. 
Les questions de Gemini étaient très pertinentes et le résultat de la spec me paraissait correct : 

```
Spécification Fonctionnelle : Timer Pomodoro

1. Introduction

Ce document spécifie les exigences pour un timer Pomodoro basé sur navigateur, implémenté en HTML, JavaScript et CSS. L'application permet à l'utilisateur de gérer des sessions de travail et de pause avec des temps personnalisables.

2. Fonctionnalités

    Démarrage Automatique : Le timer démarre automatiquement au chargement de la page.
    Modes Travail/Pause : Le timer alterne entre deux modes :
        Travail : Durée par défaut de 45 minutes.
        Pause : Durée par défaut de 10 minutes.
    Personnalisation des Temps : L'utilisateur peut modifier les durées des modes travail et pause via des champs de saisie. Il n'y a pas de limites minimales ou maximales pour ces durées.
    Validation des Temps : Un bouton unique permet de valider les temps de travail et de pause saisis.
    Affichage du Temps : Le temps restant est affiché au format MM:SS (minutes:secondes), ne dépassant pas 59:59.
    Changement de Mode : Un bouton (interrupteur) permet de basculer manuellement entre les modes travail et pause. Le basculement est également automatique à la fin de chaque période.
    Indication Visuelle du Mode : Le mode actuel est indiqué par :
        Un texte clair ("Travail" ou "Pause").
        Une icône (bureau/ordinateur pour travail, café pour pause).
    Notifications :
        Notifications du système d'exploitation :
            "Temps de travail terminé !" à la fin du mode travail.
            "Temps de pause terminé !" à la fin du mode pause.
        Titre de l'onglet : Le titre de l'onglet alterne entre "Pomodoro" et "NEXT" à une fréquence constante jusqu'à ce que l'utilisateur interagisse avec l'onglet.

3. Exigences Non Fonctionnelles

    Technologie : HTML, JavaScript, CSS (côté client uniquement, pas de serveur).
    Style : Minimaliste, moderne, clair.
    Mise en Page :
        Le temps est affiché au centre de la page.
        Les champs de saisie pour modifier les temps sont situés en dessous, également centrés.
        Les zones de modification pour le travail et la pause sont visuellement distinctes (bordure, en-tête).
    Police : Sans serif.

4. Architecture

L'application sera une page HTML unique. JavaScript gérera la logique du timer, la manipulation du DOM (Document Object Model) pour l'affichage et les interactions utilisateur, et l'envoi des notifications. CSS sera utilisé pour la mise en forme.

5. Détails du Traitement des Données

    Stockage des Temps : Les temps de travail et de pause personnalisés seront stockés en variables JavaScript.
    Mise à Jour du Timer : La fonction setInterval() de JavaScript sera utilisée pour mettre à jour l'affichage du temps restant à intervalles réguliers (par exemple, toutes les secondes).
    Gestion des Modes : Une variable indiquera le mode actuel (travail ou pause). Cette variable sera mise à jour lors du basculement manuel ou automatique.

6. Gestion des Erreurs

    Entrée Utilisateur : Bien qu'il n'y ait pas de limites aux temps, le code doit gérer les entrées non numériques ou vides dans les champs de saisie. Une valeur par défaut (ou un message d'erreur) doit être utilisée dans ces cas.
    Notifications : Le code doit vérifier si le navigateur prend en charge les notifications du système d'exploitation et fournir une alternative (par exemple, une alerte) si ce n'est pas le cas.

7. Plan de Test

    Tests Unitaires : Tester les fonctions JavaScript individuelles (par exemple, la fonction de mise à jour du timer, la fonction de basculement de mode).
    Tests d'Intégration : Tester l'interaction entre les différents composants (par exemple, la mise à jour de l'affichage du temps après la modification des temps de travail/pause).
    Tests Fonctionnels :
        Vérifier que le timer démarre automatiquement.
        Vérifier que le timer alterne correctement entre les modes travail et pause.
        Vérifier que les temps peuvent être personnalisés et validés.
        Vérifier que le temps est affiché correctement.
        Vérifier que le changement de mode fonctionne manuellement et automatiquement.
        Vérifier que les notifications sont affichées correctement.
        Vérifier que le titre de l'onglet clignote correctement.
        Tester avec différentes valeurs d'entrée (valides et invalides).
    Tests d'Interface Utilisateur : Vérifier que l'interface est conforme aux exigences de style et de mise en page.
    Tests de Compatibilité : Tester sur différents navigateurs (Chrome, Firefox, Safari, etc.) pour assurer la compatibilité.

8. Livrables

    Fichiers HTML, CSS et JavaScript de l'application.
```

En continuant sur les étapes proposées par Harper Reed, je lui ai demandé de générer les prompts qui correspondent aux itérations d'implémentation. 
Le résultat : 

```
Prompt 1.1:
Create three files: index.html, style.css, and script.js.
In index.html, add the basic HTML boilerplate (head, body, title).
Link style.css and script.js to index.html.

Prompt 1.2:
In index.html, inside the body, create a <div> element with the id "timer".

Prompt 1.4:
In style.css, add CSS rules to:
- Reset default margins and padding.
- Center the "timer" div horizontally and vertically on the page.
- Style the font of the "timer" to be large and sans-serif.

Prompt 2.1:
In script.js, declare two variables:
- `workDuration` and initialize it to 45 * 60 (seconds).
- `pauseDuration` and initialize it to 10 * 60 (seconds).
Also, declare a variable `timeLeft` and initialize it to `workDuration`.

Prompt 2.2:
In script.js, implement a function named `countdown`.
Inside `countdown`:
- Decrement `timeLeft` by 1.
- Calculate minutes and seconds from `timeLeft`.
- Format the time as "MM:SS".
- Update the content of the "timer" div with the formatted time.

Prompt 2.3:
In script.js, use `setInterval` to call the `countdown` function every 1000 milliseconds (1 second).


Prompt 2.4:
In index.html, add two new <div> elements below the "timer" div:
- One with the id "mode-text" to display the current mode (e.g., "Travail").
- Another with the id "mode-icon" to display an icon (use a placeholder for now, e.g., "icon").

Prompt 2.5:
In script.js, when the page loads, set the content of "mode-text" to "Travail" and add a placeholder text "work icon" to "mode-icon".

Prompt 2.6:
In index.html, add a <button> element with the id "mode-toggle" below the "mode-text" and "mode-icon" divs. Set its initial text to "Switch to Pause".

Prompt 2.7:
In script.js, add an event listener to the "mode-toggle" button.
When clicked:
- If the current text of "mode-text" is "Travail", change it to "Pause", change the button text to "Switch to Work", and update timeLeft to pauseDuration.
- Otherwise, change the text of "mode-text" to "Travail", change the button text to "Switch to Pause", and update timeLeft to workDuration.

Prompt 3.1:
In index.html, add two <input> elements below the "mode-toggle" button:
- One with the id "work-duration" and type "number".
- Another with the id "pause-duration" and type "number".

Prompt 3.2:
In index.html, add <label> elements for the "work-duration" and "pause-duration" input fields, clearly indicating what they are for.

Prompt 3.3:
In index.html, add a <button> element with the id "validate-times" below the input fields and labels. Set its text to "Validate".

Prompt 3.4:
In script.js, add an event listener to the "validate-times" button.

Prompt 3.5:
Inside the "validate-times" event listener, get the values from the "work-duration" and "pause-duration" input fields.


Prompt 3.6:
Inside the "validate-times" event listener, convert the input values to numbers using `parseInt()`.
- Use `isNaN()` to check if the values are not numbers.
- If a value is not a number or is empty, use default values (45 for work, 10 for pause).
- Store the parsed values in temporary variables.

Prompt 3.7:
Inside the "validate-times" event listener, update the `workDuration` and `pauseDuration` variables with the validated input values (or the defaults).

Prompt 3.8:
Inside the "validate-times" event listener, reset `timeLeft` to `workDuration` and update the timer display.

Prompt 4.1:
Modify the `countdown` function in script.js to:
- Check if `timeLeft` is 0.
- If it is, switch the mode (if it was "Travail", switch to "Pause", and vice versa).
- Reset `timeLeft` to the appropriate duration (`workDuration` or `pauseDuration`).

Prompt 4.2:
Inside the `countdown` function, update the text of "mode-text" and the placeholder text of "mode-icon" when switching modes automatically (e.g., "Pause", "pause icon").

Prompt 4.3:
In script.js, outside any function, check for browser notification support:
- Use `Notification.requestPermission()` to request permission to send notifications.
- Use `window.Notification` to check if notifications are supported.

Prompt 4.4:
In script.js, implement a function named `showNotification` that takes a message as an argument.
- Inside `showNotification`, create a new `Notification` object with the given message.

Prompt 4.5:
In the `countdown` function, after switching modes automatically, call `showNotification` with the appropriate message ("Temps de travail terminé!" or "Temps de pause terminé!").

Prompt 4.6:
In script.js, if notifications are not supported (checked in Prompt 4.3), provide an `alert()` fallback for the notifications in the `countdown` function.

Prompt 5.1:
In script.js, implement a function named `flashTitle`.
- Inside `flashTitle`, check the current document title.
- If it's "Pomodoro", set it to "NEXT".
- Otherwise, set it to "Pomodoro".

Prompt 5.2:
In script.js, use `setInterval` to call `flashTitle` every 1000 milliseconds.
Store the interval ID in a variable.


Prompt 5.3:
In script.js, add an event listener to the `document` for a user interaction event (e.g., "click" or "keydown").
- Inside the event listener, use `clearInterval` with the interval ID to stop the title flashing.

Prompt 5.4:
Refine the CSS in style.css to achieve the final desired style:
- Minimalist design.
- Modern fonts and colors.
- Clear layout and spacing.
- Visually distinct sections for timer, input, etc.
- Style the mode icons (you may need to use a library or Unicode characters).

Prompt 5.5:
Thoroughly test the application:
- Manually test all functionalities.
- Use browser developer tools to check for errors.
- Test on different browsers (Chrome, Firefox, Safari).
- Test with various input values (valid and invalid).


Prompt 5.6:
Fix any bugs identified during testing and polish the application's appearance and functionality.
```

J'ai ensuite entré ces prompts tous d'un coup dans Gemini et demandé de les implémenter. 
Le résultat a été fonctionnel immédiatement. 
Après quelques ajustements, je me suis retrouvé avec un timer fonctionnel qui répond à mes besoins spécifiques. 
Si vous voulez le voir [c'est ici sur mon site](https://davidfranck.eu/timer/). 
Tout ça en une trentaine de minutes. 

Je l'utilise depuis quelques jours, et ça marche bien. 
L'objectif initial est rempli. 
J'applique plus mes phases, je prends bien mes pauses. 
A voir dans la durée, bien entendu. 