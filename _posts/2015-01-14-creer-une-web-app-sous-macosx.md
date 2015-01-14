---
layout: post-no-feature
title: "Créer une web app sous Mac OS X"
description: "Exploitez Google Chrome avec cette petite astuce"
category: articles
comments: true
tags: [web, app, osx, google, chrome, messages]
---

![remote-messages]({{ site.url }}/images/remote-messages.png)

Si comme moi vous utilisez quelques web apps de manière quotidienne, pouvoir
bénéficier d'un raccourci sympa qui nous fasse une application en full-screen
serait bien agréable. Suivez le guide !

## Exploitons une ligne de commande de Google Chrome

On sait qu'il est possible de lancer Google Chrome en spécifiant une URL
spécifique :

{% highlight bash %}
exec '/Applications/Google Chrome.app/Contents/MacOS/Google Chrome' --app="http://www.google.com"
{% endhighlight %}

On sait aussi qu'on peut bundler dans une application des scripts bash, soit
avec *Automator*, soit à la main, en mettant le script dans une application
de telle sorte à avoir */Applications/Mon App.app/Contents/MacOS/Mon App*.

## Automatiser la création de raccourcis

On va utiliser un petit script Automator nommé
[CreateCgApp](https://s3.amazonaws.com/LACRM_blog/createGcApp.dmg) qui permet de
facilement donner un nom au raccourci, l'URL souhaitée ainsi que l'icône,
histoire que ça soit pas trop moche dans le dock.

1. Renseignez le nom du raccourci

![web-app-1]({{ site.url }}/images/web-app-1.png)

2. Renseignez l'URL

![web-app-2]({{ site.url }}/images/web-app-2.png)

3. On sélectionne une jolie icone au format icns.. et voilà !

![remote-messages-dock]({{ site.url }}/images/remote-messages-dock.png)

Dans mon cas, j'utilise souvent Remote Messages qui est une interface web sur
iOS pour envoyer/recevoir des sms depuis l'iPhone, un peu à la manière de
Messages avec Continuité.

Pour tous ceux ne pouvant pas le faire (ou n'étant pas sous Yosemite ni iOS
8.1), Remote Messages est un tweak disponible sous Cydia très utile. Avec une
petite icône sur le dock et une web app dans Chrome, plus d'excuses ;)
