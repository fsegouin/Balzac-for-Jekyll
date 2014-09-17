---
layout: post-no-feature
title: "Devenir un gourou du terminal"
description: "Il fait peur, mais c'est bientôt votre nouveau meilleur ami."
category: articles
comments: true
tags: [mac, osx, unix, terminal, vim, zsh, oh-my-zsh]
---

![iterm]({{ site.url }}/images/yosemite-iterm.png)

> “C'est quoi ce truc de geek ?”

Le Terminal sera bientôt votre nouveau meilleur ami : si vous ne le connaissez pas
encore, il est grand temps de vous y pencher, mais je ne vais pas vous laissez
seuls face à cette ligne de commande qui fait si peur. D'ailleurs, moi-même je
ne l'utilisais que peu avant de découvrir tous les paquets dont je vais vous
parler dans la suite, et qui transformeront ce vieil ennemi en votre nouveau
partenaire de tous les jours.

> Suivez le guide !

## Exit Bash, bonjour zsh

Dans la famille des [shell Unix](http://fr.wikipedia.org/wiki/Shell_Unix)
(comprendre : interpréteur de commandes), Bash (Bourne-Again shell, jeu de mot
par rapport au Bourne shell, le shell historique d'Unix), est celui installé par
défaut dans OS X. Il n'est pas le meilleur, mais il n'est pas non plus le plus
horrible, il conviendra à tous pour une utilisation ponctuelle. Cependant, si
vous décidez de passer le cap du Terminal, il est fortement recommandé (et je
l'ai adopté) de passer à zsh (pour Z Shell) qui comporte beaucoup
d'améliorations par rapport à Bash, dont : 

* Autocomplétion des commandes, options et arguments.
* Historique des commandes à travers tous les shells zsh actifs.
* etc

On va notamment le coupler à *oh-my-zsh* qui est un framework pour gérer votre
configuration zsh. Commençons donc par installer celui-ci :

{% highlight bash %}
➜  ~ curl -L http://install.ohmyz.sh | sh
{% endhighlight %}

Si tout va bien, *oh-my-zsh* devrait directement définir zsh comme shell par
défaut pour votre profil actuel. Fermez le terminal et relancez-le : vous voilà
sous zsh. Vous devriez déjà noter quelques différences, qu'on va encore pousser
un peu plus.

Il est temps d'installer un thème sympa. Pour cela, il suffit d'éditer le
fichier *~/.zshrc*. Deux choses à noter :

* Si vous souhaitez vous débarasser du préfixe user@machine dans le prompt, il
suffit d'ajouter *DEFAULT_USER="votre-user"*.

* Pour modifier le thème on va définir le thème souhaité dans *ZSH_THEME="theme"*
en se basant sur la [liste de thèmes disponibles](https://github.com/robbyrussell/oh-my-zsh/wiki/themes).

> Personnellement, ma préférence va à Agnoster.

![agnoster]({{ site.url }}/images/yosemite-agnoster.png)

Avant d'aller plus loin, il est temps de dire au revoir au Terminal de OS X et
d'utiliser l'excellent [iTerm2](http://iterm2.com/) qui vous permettra de
customizer plus facilement le terminal.

Si vous souhaitez également utiliser le thème *agnoster* sous iTerm2, il est
nécessaire d'utiliser une [police patchée pour
Powerline](https://www.dropbox.com/s/p4v92lky78jw4i6/Screenshot%202014-09-16%2019.51.25.png?dl=0).
Vous pouvez utiliser Meslo, qui est un dérivé de Menlo, la police utilisée par
Apple dans Xcode. Cela permettra d'afficher les caractères ASCII qui forment les
chevrons dans le prompt. 

![iterm-fonts]({{ site.url }}/images/iterm-fonts.png)

Pour finir, je vous conseille d'utiliser l'excellent thème
[Solarized](http://ethanschoonover.com/solarized) par Ethan Schoonover.
Disponible en *Light* et *Dark*, les fichiers de configuration pour iTerm2 sont
disponibles dans le pack. Il vous suffira d'aller dans *Preferences > Colors >
Load Presets...* et d'importer la version de Solarized qui vous convient. La
version Dark est juste sublime avec le thème zsh *agnoster* !

### Encore un peu plus de #swag

Il est temps d'ajouter de la couleur dans nos lignes de commandes.

![zsh-syntax-highlighting]({{ site.url }}/images/zsh-syntax-highlighting.png)

Téléchargez le plugin
[zsh-syntax-highlighting](https://github.com/zsh-users/zsh-syntax-highlighting)
dans le dossier plugins de oh-my-zsh en closant le repo git du plugin :

{% highlight bash %}
➜  ~ cd ~/.oh-my-zsh/custom/plugins
{% endhighlight %}

On active le plugin dans notre fichier de configuration *~/.zshrc* comme ceci :

{% highlight bash %}
plugins=(git zsh-syntax-highlighting)
{% endhighlight %}

On prend en compte les changements directement (ou vous redémarrez le terminal) :

{% highlight bash %}
➜  ~ source ~/.zshrc
{% endhighlight %}

## Vim

Vous commencez à vous familiariser avec le Terminal, mais il est temps
d'améliorer Vim. Cet excellent éditeur de texte en ligne de commandes est juste
indispensable pour tout bon utilisateur du Terminal. Seulement voilà, il est pas
très sexy dans sa version d'origine, et il aurait bien besoin d'un petit coup de
jeune. Pour vous faciliter la tâche, [spf13](http://spf13.com) a compilé une
[distribution Vim géniale](http://vim.spf13.com/#install) qui intègre
[vim-airline](https://github.com/bling/vim-airline), une *statusbar* pour Vim
qui le rend excellent.

![vim-airline-gif](https://github.com/bling/vim-airline/wiki/screenshots/demo.gif)

> spf13-vim is a distribution of vim plugins and resources for Vim, GVim and MacVim.

L'installation se fait en une ligne :

{% highlight bash %}
➜  ~ curl http://j.mp/spf13-vim3 -L -o - | sh
{% endhighlight %}

Avant d'aller plus loin, petit rappel pour les néophytes. Ouvrez un fichier avec
*vim monfichiertexte* et commencez l'édition avec la touche *i*. Vim repose sur
un mode insertion et un mode lecture. Sortez du mode édition avec la touche
*échap*. Chaque commande commence avec les deux points *:* et *wq* vous permet
de *write & quit*.

Pour avoir les chevrons dans la nouvelle ligne de status de Vim, il vous suffit
d'ajouter les lignes suivantes dans votre *~/.zshrc* :

*export LC_ALL=en_US.UTF-8* et *export LANG=en_US.UTF-8*

On y est presque ! Créez le fichier *~/.vimrc.before.local* et ajoutez-y la
ligne suivante : *let g:airline_powerline_fonts=1*.

> C'est maintenant l'heure de dire bonjour à votre nouvel éditeur de texte favori.

![spf13-vim]({{ site.url }}/images/spf13-vim.png)

La statusbar vous permet désormais de visualiser dans quel mode vous vous
situez, mais aussi dans quelle branche de votre repo vous travaillez, le numéro
de ligne, etc.

## Enfin prêts ?

J'espère que ce guide vous aura permis de découvrir **mon** environnement de
travail et qu'il vous aura inspiré pour améliorer le votre. Il y a encore
quelques mois je découvrais le terminal.. Aujourd'hui je ne peux plus m'en
passer. Il a remplacé mes éditeurs de texte, le Finder, et j'en passe :)
