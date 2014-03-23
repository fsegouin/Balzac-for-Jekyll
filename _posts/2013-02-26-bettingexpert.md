---
layout: post-no-feature
title: "bettingexpert sur iOS"
description: "Choper un TN09 en 1 article, c'est facile."
category: articles
comments: true
tags: [iOS, programming, tech]
---

*Mise à jour (23/03/14) :*

C'est assez drôle de mettre à jour un article plus d'un an après sa rédaction. Mais c'est aussi ce qui me permet de vous dire que, suite à cet article et cette prise de contact par le CSO de [Better Collective](http://www.bettercollective.com), j'ai effectué mon premier stage de cycle ingénieur (TN09) chez eux! L'application est désormais disponible sur l'AppStore. Alors si toi aussi t'aimes parier ou si t'aimes tout simplement le football et les statistiques, cette [app](https://itunes.apple.com/fr/app/bettingexpert-free-betting/id778618162?mt=8) est faite pour toi, clique clique!

*Mise à jour (08/03/13) :*

La magie de Google ayant fait son effet, un responsable de chez [bettingexpert](http://www.bettingexpert.com) m’a contacté sur Twitter pour avoir plus d’informations sur l’application. Peut-être mes débuts sur l’AppStore dans les prochains jours ? ;)

## Allez, raconte-nous tout !

Pour occuper mon inter-semestre à l’UTT, j’ai décidé de progresser un peu en Objective-C en programmant pour iPhone. J’avais déjà fait une application pour une société un été il y a 2 ans, mais le SDK de l’iOS évoluant tellement vite, on se retrouve souvent à recommencer à apprendre à zéro. Entre l’apparition des storyboards et des segues, c’est encore plus agréable de programmer sur cette plateforme. C’est devenu un jeu d’enfant de relier les view entre elles et de se passer les objets. Bref, j’avais vraiment envie de mettre sur pieds quelques projets qui me trottaient en tête.

« bettingexpert » est une application destinée aux amateurs de paris sportifs. Elle récupère les données du site bettingexpert.com et propose très rapidement tous les tips pour chaque match, football ou basketball (plus à venir).

![Capture 1]({{ site.url }}/images/bettingexpert-ios-2.png)

> Oui, c'est honteusement moche. Heureusement qu'il y a eu du chemin parcouru depuis.

Cela fait déjà plusieurs semaines que je m’étais mis en quête de chercher une bonne librairie pour du parsing HTML efficace sous Objective-C. Le site bettingexpert ne donnant rien de très utile dans son flux RSS, sans même l’ombre d’un éventuel service json, il a fallu mettre les main dans le cambouis et se servir soi-même ! Pour ça, github.com est vraiment un très bon site, on y trouve souvent LA librairie qui va nous sauver. J’ai jeté mon dévolu sur la très bonne librairie HTMLDocument de Stefan Klieme, récemment mise à jour pour être compatible avec ARC, bref du tout bon.

Une fois passé cette étape, il faut analyser comment récupérer les données qui nous intéressent. Ici concrètement pour chaque match, le nom des deux équipes, la date, la ligue. Pour chaque match, on va faire un tableau de « tips » avec, pour chaque tip, le nom du tipster, ses stats, son choix, la côte et les units qu’il place. On jette un coup d’oeil au code html de la page concernée et on observe ça :

![Capture 2]({{ site.url }}/images/bettingexpert-ios-7.png)

Pour chaque classe « match » on va avoir les attributs suivants : href (lien du tip), title (nom des équipes) et en texte du noeud on aura le nom du match suivi du nom de la ligue. Pour la classe « time » on récupère directement le texte. Idem pour les classes tipster, tipsterInfo, selection, odds et stake. On va pouvoir créer nos objets Game et Tip avec tous nos attributs. Les tips seront stockés dans un NSMutableArray, variable de l’objet Game. Il suffit ensuite de créer nos instances de NSTableViewController pour pouvoir afficher tout ça sous la forme d’une TableView. Les matches seront listés, par date, puis la sélection d’un match amène sur un tableau récapitulatif des tips pour ce dernier.

![Capture 3]({{ site.url }}/images/bettingexpert-ios-3.png)

Si le match est en rouge dans la liste, c’est qu’il est considéré comme « populaire », ayant plus de 5 tips. Dans l’écran récapitulatif, pour plus de visibilité, on met en rouge les tendances. Les stakes quand à elles sont de couleur rouge, orange ou vert selon leur degré de probabilité. On peut également directement accéder au site web (UIWebView) pour lire le descriptif du tip en le sélectionnant dans la liste.

![Capture 4]({{ site.url }}/images/bettingexpert-ios-4.png)

Derrière les jolis screenshots ça donne quoi ?

![Capture 7]({{ site.url }}/images/bettingexpert-ios-6.png)

> C'est joli les storyboards mais c'est quand même sacrément dégueulasse, surtout quand c'est la première fois que tu les utilise.

Le nouveau Interface Builder sous Xcode 4.5 est tout simplement un délice ! Sans parler des storyboards : il suffit désormais de relier les vues entre elles et de nommer les segues pour gérer en à peine plus de 5 lignes de code ce qu’on devait faire auparavant en plusieurs méthodes. Autre avantage et non des moindres : on a maintenant un aperçu visuel de l’enchaînement de nos vues ! Ça peut paraître minime, mais sur une application à plus d’une vingtaines de contrôleurs de vue, pouvoir identifier les liens en 1 seul coup d’oeil, c’est magique. Surtout quand on reprend son code 1 an après…

Bref, voici une belle petite application pour les amateurs de sport ! Les sources seront bientôt disponibles sur mon github. N’ayant pas de compte développeur, je ne mettrai pas l’application sur l’AppStore. Cependant, avec les sources, il vous sera très facile de compiler l’app et l’auto-signer si vous êtes jailbreakés, ou la signer avec votre certificat si vous êtes possesseurs d’un compte développeur.

Pour résumer : si vous voulez également concevoir une app native sur iOS qui s’appuie sur un site web, ce parser HTML est fait pour vous ! Un petit bijoux plutôt rapide qui fait bien le boulot. Evidemment si vous avez du rss ou du json sous le coude, c’est encore mieux.. ;)

## Résultat

<iframe width="420" height="315" src="//www.youtube.com/embed/vDxHRk10RE0?rel=0" frameborder="0"></iframe>

Une petite vidéo pour vous faire une idée de l’application à l’heure actuelle (edit : c'est à dire.. il y a fort longtemps).