---
layout: post-no-feature
title: "Installer htop sous OSX Moutain Lion (10.8)"
description: "Et si on amenait un peu de linux sur nos OSX? Ca commence par htop."
category: articles
comments: true
tags: [osx, programming, unix]
---

![htop]({{ site.url }}/images/htop-screenshot.png)

Un des premiers trucs que j’installe en général sur une nouvelle machine sous unix c’est htop : le petit gestionnaire de processus magique. Seulement, loin de faire comme tout le monde, Apple nous complique un peu la tâche : pas de apt-get sous OSX. Il n’y a même pas de binaires compilés sur le site officiel de htop, c’est pour dire. Heureusement sous OSX pour palier à l’absence d’un bon gestionnaire de paquet (à défaut d’apt) on a [Homebrew](http://brew.sh/). Pas besoin d’en faire des tonnes, la page d’accueil donne déjà le ton : « Homebrew installs the stuff you need that Apple didn’t ».

Le truc sympa avec Homebrew, c’est qu’en plus d’être un gestionnaire de paquet, c’est surtout un gestionnaire de « formules ». Entendez par là que chaque paquet que vous allez installer possède sa propre formule (comprendre script d’installation). Comme rien n’est vraiment facile sous OSX pour installer des paquets, chaque formule est adaptée de manière à ce que tout soit automatique pour l’utilisateur.

Avant d’aller plus loin, Homebrew aura besoin, pour compiler depuis les sources, des « Command Line Tools » disponibles directement depuis Xcode (Preferences > Downloads). Si vous ne souhaitez pas installer Xcode juste pour ça, ils sont aussi disponibles en paquet séparé sur le site d’Apple, section développeur.

## Installation

{% highlight bash %}
brew update && brew doctor
Already up-to-date.
Your system is ready to brew.
{% endhighlight %}

On commence par tout mettre à jour et on vérifie qu’il n’y a aucun soucis de compilateur pas à la bonne version ou mal installé. Tant que brew ne vous annonce pas que tout est à jour et que le système est « ready to brew », pas la peine d’aller plus loin. Si soucis il y a, suivez les [instructions](https://github.com/mxcl/homebrew/wiki/troubleshooting), c’est en général assez clair.

{% highlight bash %}
brew install --use-gcc htop-osx
{% endhighlight %}

On demande d’installer le paquet htop-osx (fork de htop sous github pour osx) en précisant d’utiliser gcc pour la compilation car ça ne passe pas sous clang (l’alternative à gcc made in Apple proposée d’office depuis quelques versions).

Il se peut que vous rencontriez une erreur de ce style lors de l’installation des dépendances de htop :

{% highlight bash %}
error: C++ preprocessor "/lib/cpp" fails sanity check
{% endhighlight %}

Pas de panique, c’est juste un configure pas très intelligent qui n’a pas compris qu’il était sous OSX : le chemin /lib n’existe pas. Deux solutions, soit on le précise au compilateur, soit on s’embête pas trop et on va créer un lien symbolique entre /lib/cpp et /usr/bin/cpp.

{% highlight bash %}
sudo ln -s /usr/bin/cpp /lib/cpp
{% endhighlight %}

Logiquement par la suite tout devrait bien se dérouler.

Vous remarquerez que htop sous OSX n’affiche tous les détails des processus du système que s’il est lancé en étant root. Deux solutions encore une fois. Un « sudo htop » permettra de le lancer avec les droits mais c’est un peu contraignant. On peut aussi s’attaquer au problème directement à la source en lui donnant les droits de sorte que n’importe quel user qui lancera htop lancera en réalité le processus avec les droits de root ([source](http://logicbox.tumblr.com/post/17067077640/installing-htop-on-osx-lion-with-homebrew)).

{% highlight bash %}
cd /usr/local/Cellar/htop-osx/0.8.2.1/bin
chmod 6555 htop
sudo chown root htop
{% endhighlight %}

Et voilà! Il n’y a plus qu’à profiter de htop sous nos jolis Mac :)