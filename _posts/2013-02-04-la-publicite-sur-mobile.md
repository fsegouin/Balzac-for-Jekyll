---
layout: post-no-feature
title: "La publicité sur mobile"
description: "Je ne suis pas dans la com, et fort heureusement!"
category: articles
comments: true
tags: [iOS, security, tech, advertising]
---

Qu’on se le dise : la publicité sur mobile est véritablement sur une pente ascendante et ça n’est pas prêt de s’arrêter. Si vous pensiez déjà en voir trop, vous n’allez pas être déçus, il va y en avoir encore plus ! Tout cela pour une raison bien simple, toute l’économie du web est bouleversée par les terminaux mobiles nouvelle génération, ces smartphones où l’on peut installer à peu près tout et n’importe quoi. Les annonceurs commencent à y jeter un oeil plus qu’intéressé et les investissements décollent. Bref aujourd’hui, que l’on soit développeur indépendant ou grosse société, que l’on développe un jeu free-to-play ou une application de service, un bon moyen de fidéliser le consommateur et d’en tirer des revenus, c’est bien d’y installer des bannières publicitaires. C’est facile : on gagne de l’argent à l’affichage mais également au clic, et il y a souvent une très jolie règle dans ce domaine : plus on occupe l’écran et plus on reste longtemps, plus ça paye ! Reste que maintenant, beaucoup n’ont aucun scrupule et ne cherchent plus vraiment le bon rapport entre ergonomie et affichage de publicité. On en met à toutes les sauces, si possibles qui volent, qui clignotent, qui reviennent même quand on les ferme, bref retour au web des années 90 et ses pop-ups.

Oui mais voilà : sur un terminal mobile, on peut pas toujours gérer tout ce qui s’ouvre, et encore moins le fermer. La plupart du temps, comme dans de nombreux sites web, les bannières de publicité sont incluses dans les sites web, vous laissant un bon gros cm de pub sur un écran déjà pas très grand. Résultat : ça gêne la lecture, ça perturbe l’attention, enfin bref moi les publicités sur mobile, ça commence à devenir mon combat favori, bidouilleur dans l’âme que je suis.

Venons-en dans le vif du sujet : comment se débarrasser de ces publicités ? Bien sûr, il y a toujours une solution, comme on va le voir, mais est-elle vraiment éthique ? La plupart du temps, le service fourni est directement financé par l’ajout de ces publicités qui font vivre le développeur ou la boîte qui fourni le service. Bref, chacun sa vision de la chose, mais soyez conscient que plus elle sera difficile à mettre, et plus ils en mettront…

## Comment s’en débarrasser ?

Si comme moi vous aimez bien mettre les mains dans le cambouis et découvrir les rouages de vos applications favorites, suivez le guide !

Tout d’abord, il faut savoir que la plupart du temps, ces publicités sont hébergées chez des régies de publicité telles que AdMob, iAd, SmartAdServer, etc. Pour pouvoir empêcher l’affichage de ces publicités, on va devoir tout simplement bloquer les requêtes de l’application vers les serveurs concernés. On pourrait très bien faire une liste de toutes les adresses, mais c’est quand même plus amusant de la personnaliser pour les applications que l’on utilise (et au moins, on sera sûr de tout enlever).

Pour cela, il va nous falloir un logiciel qui puisse analyser le trafic entrant et sortant de notre smartphone. Je vous conseille pour cela les excellents [Fiddler](http://www.fiddler2.com/fiddler2/) (windows only) ou [Paros](http://www.parosproxy.org/) (multi-plateformes, en Java). Grâce à ces derniers, on va pouvoir mettre en place un proxy et ainsi intercepté et modifier tout le trafic.

J’ai effectué la suite de ce tutorial sur Paros, étant sous MacOSX.

Allez dans : Tools > Options > Local Proxy afin de configurer un proxy. Pour l’adresse, mettez votre IP locale (sur le réseau, ici pour moi 192.168.0.112), et non pas localhost comme indiqué, sinon il ne sera pas accessible depuis votre smartphone. Pour le port, laissez 8080.

![Options Paros]({{ site.url }}/images/screenshot-paros-1.png)

Côté smartphone (ici sur un iPhone) on ajoute un proxy pour la connexion sur les mêmes critères que précédemment.

![Proxy iOS]({{ site.url }}/images/proxy-ios.png)

Désormais, tout le trafic entrant et sortant de l’iPhone s’affichera dans la fenêtre principale de Paros. Lancez vos applications favorites, laissez-les afficher les publicités et revenez sur la fenêtre de Paros pour voir tout ce qu’on a capturé.

## Un peu d’analyse

### Avec l’application MacGeneration :

![Capture 1]({{ site.url }}/images/screenshot-paros-2.jpg)

On utilise une seule régie publicitaires ici, feedvalue.com, ainsi que flurry.com chez qui ils envoient (requête POST) des données utiles sur leurs utilisateurs. Si l’on passe sur le fait que feedvalue.com n’est qu’une régie publicitaire tout ce qu’il y a de plus classique (et déjà une jolie adresse à bloquer par la suite), intéressons nous de plus prêt aux données qui sont émises chez flurry :

![Capture 2]({{ site.url }}/images/screenshot-paros-3.jpg)

On retrouve entre autres : un numéro de série, le device ID2, taille de l’écran, version de l’iOS, langue et fuseau horaire de l’iPhone, où on est situé dans l’application et quel article est-on en actuellement en train de lire. Même si c’est (sûrement) très utile pour les développeurs, je trouve ça un poil trop détaillé. Bref, direction la blacklist !

### Avec l’application Le Monde (attention on lésine pas sur les requêtes) :

![Capture 3]({{ site.url }}/images/screenshot-paros-4.jpg)

C’est la fête aux régies publicitaires chez Le Monde, on retrouve du GoogleAds, du adMob, du SmartAdServer… Vous noterez au passage qu’on a la liste magique des fichiers json parsés pour afficher les données dans l’application. Si vous avez envie de refaire votre propre application Le Monde en plus sympa et sans pubs, servez-vous, ya aucun token, aucune sécurité, c’est open bar ! C’est assez drôle d’ailleurs, les fichiers json sont récupérés dans un chemin qui comporte « android »…

### Avec l’application l’Equipe :

![Capture 4]({{ site.url }}/images/screenshot-paros-5.jpg)

On retrouve du SmartAdServer, et encore une fois toute la liste des fichiers json parsés. Encore une fois, rien n’est protégé pour l’accès, si vous voulez refaire votre livescore maison, faites comme chez vous !

Chez SmartAdServer on est un peu moins gourmand que chez Flurry niveau données recueillies, ici ça reste simple :

{% highlight bash %}
jsonMessage={« prefetch »:true, »language »: »fr », »connexion »: »wifi », »version »: »3.3″, »platform »: »iPhone5,2″}
{% endhighlight %}

## Conclusion

Une fois que vous avez la liste de tous les domaines à bloquer (typiquement tous les domaines que j’ai entouré en rouge), il vous reste deux solutions. Si vous êtes sur Android et iOS (jailbreaké) vous pouvez facilement éditer le fichier « hosts » du système, c’est comme une table de routage mais elle est toujours prioritaire par rapport à celle donnée par votre routeur. Il est donc intéressant de s’en servir afin de rediriger des domaines vers le localhost pour purement et simplement bloquer toute connexion à ceux-ci. Pour ceux qui sont sur iOS et qui ne sont pas jailbreakés, vous pouvez monter un petit serveur proxy sous Linux ou encore laisser tourner celui de Paros ou Fiddler en utilisant les règles de blocage pour facilement faire une liste d’adresse à filtrer. Petit bémol cependant, selon les applications, certaines sont codées de manière à prévoir l’emplacement de la publicité « en dur » dans l’interface. Concrètement, vous aurez un beau bandeau blanc (ou noir) à la place de la publicité. Résultat, on gagne pas forcément en lisibilité mais au moins ça gênera moins. D’autres sont plus intelligentes, si elles n’ont pas de réponse du serveur, elles n’afficheront tout simplement rien.

Encore une fois, chacun son avis sur le modèle économique mis en place dans les applications, libre à vous de ne pas bloquer la publicité pour les applications que vous appréciez. Pour le reste, vous savez ce qu’il vous reste à faire ;)