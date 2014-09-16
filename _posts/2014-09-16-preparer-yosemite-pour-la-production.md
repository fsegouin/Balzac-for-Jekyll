---
layout: post-no-feature
title: "Préparer Mac OS X Yosemite 10.10 pour la production"
description: "Yosemite arrive, et il est temps de préparer nos machines."
category: articles
comments: true
tags: [mac, osx, unix, yosemite, terminal, vim, ruby, nodejs]
---

![iterm]({{ site.url }}/images/yosemite-iterm.png)

Yosemite vient tout juste d'être mis à jour hier (15/09/14) en Developer Preview
8 et Public Beta 3. Il se murmure que la version finale, dont la sortie est
prévue pour Octobre 2014 ne serait désormais plus très éloignée des versions
beta dont nous disposons à l'heure actuelle, il est donc temps de faire le point
sur ce qu'il faudra (encore) réinstaller lorsque Yosemite débarquera en version
finale.

> “Est-ce bien sérieux de préparer sa prod sur une version beta ?”

Oui et non. D'un côté, ça permet aussi de tout préparer pour limiter au maximum
le temps d'indisponibilité de sa machine de prod, surtout que le passage à
Yosemite sera quasiment obligatoire pour tous les développeurs iOS souhaitant
compiler pour iOS 8. De l'autre, certains bugs devront être corrigés à l'heure
actuelle (notamment pour [installer mangodb avec
Homebrew](http://stackoverflow.com/questions/24052145/trouble-reinstalling-mongodb-with-homebrew-using-os-x-10-10-yosemite-beta))
et leurs recettes seront sûrement remises à jour, rendant obsolète un article
qui expliquerait comment corriger cela. En tous cas, cette petite check-up list
vous permettra au moins de gagner du temps pour vos prochaines installations, et
pourquoi pas de prendre bonnes habitudes de travail.. ou d'en découvrir d'autres ? :)

## Rapide tour du propriétaire

C'est plat, très plat, et c'est pas fini. Yosemite enterre définitivement le
skeuomorphisme qu'Apple avait pourtant introduit un peu partout. Une nouvelle
page se tourne... et iOS 7 avait déjà bien fait le travail un an auparavant.
C'est désormais la chasse à chaque ombre ou effet 3D. La dock est désormais
entièrement plate, et même les icones y passent. La menubar adopte une robe
entièrement blanche sans aucun effet, et sa cousine en robe noire adopte le même
principe.

### Quoi de neuf sous le capot

Yosemite se débarasse aussi de certaines reliques du passé : Ruby 1.8.7 qui
était encore présent dans Mavericks au travers du paquet *Command Line Tools* à
des fins de compatibilité est désormais purement et simplement supprimé de l'OS,
et garde pour seule version Ruby 2.0 (qu'on s'empressera de mettre à jour sans
toucher à la version fournie par Apple). Cependant, chaque bonne nouvelle
s'accompagne de son petit soucis : des développeurs fainéants avaient pris la
mauvaise habitude de *hardcoder* dans leurs installations le chemin vers la
version 1.8.7 de Ruby, au lieu de s'appuyer sur un symlink dans le path par
défaut de /usr/bin.

Heureusement, de nombreux scripts ont déjà été mis à jour et cela ne devrait
plus poser de soucis, mais dans le cas où un script un peu oublié vous pose des
soucis avec Ruby, vous savez déjà d'où cela peut éventuellement venir.

## C'est l'heure d'optimiser notre nouvel environnement de travail

Avant de commencer, il serait bon d'installer Xcode, l'IDE d'Apple, qui renferme
de nombreux paquets qui seront indispensables pour la suite (git, etc). Il est
désormais possible d'obtenir ces packets sans passer par Xcode en installant les
*Command Line Tools*. Ces derniers vous seront automatiquement proposés lors de
l'installation de Homebrew (puisque l'on clone un repot git) ou vous pouvez tout
de suite le faire avec la commande suivante :

{% highlight bash %}
➜  ~  xcode-select --install
{% endhighlight %}

### Homebrew

Homebrew est un peu l'*apt-get* de OS X. D'ailleurs, je ne peux que reprendre
leur propre description pour vous le présenter.

> Homebrew installs the stuff you need that Apple didn’t.

Homebrew permettra notamment à tous nos amis venus des distributions Linux de se
sentir un peu moins perdus. Avec un simple *brew install* vous avez 90% de
chances de retrouver vos paquets habituels.

Passions à l'installation :

{% highlight bash %}
➜  ~  ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
{% endhighlight %}

Une rapide mise à jour et une vérification du système plus tard...

{% highlight bash %}
➜  ~  brew update
➜  ~  brew doctor
{% endhighlight %}

Si *brew doctor* vous signale d'éventuelles erreurs, il est en général assez
simple de les résoudre grâce au log qu'il génère. Le cas échant, un petit tour
vers la [FAQ](https://github.com/Homebrew/homebrew/wiki/FAQ) devrait vous
aiguiller.

Libre à vous ensuite de personnaliser votre environnement avec vos paquets
habituels :

* htop
* wget 
* etc...

### Ruby

Ruby est devenu indispensable pour tous les développeurs web, qu'ils soient
back-end ou front-end. De nombreux frameworks sont construits dessus et vous ne
pourrez pas y échapper. D'ailleurs, si vous ne connaissez pas encore ce langage
de programmation qui a notamment inspiré
[Swift](https://developer.apple.com/swift/), le nouveau langage d'Apple, vous
devriez vous y pencher, ce langage va devenir dans quelques années un langage
phare (et il l'est déjà) dans le développment web.

Pour ce qui est d'installer Ruby, il y a en général deux écoles. Vous avez le
choix de l'installer avec *brew install ruby* qui en règle générale vous donnera
la dernière version stable. Cependant, il est plus intelligent d'utiliser RVM,
pour *Ruby Version Manager*, qui est, comme son nom l'indique, un gestionnaire
de versions de Ruby. C'est notamment utile si vous devez jongler entre plusieurs
versions de Ruby durant vos projets.

Pour les inconditionnels de l'interface graphique, il existe
[JewelryBox](https://jewelrybox.unfiniti.com/) qui s'occupera d'installer RVM,
Ruby et même gérer vos gems (les paquets de Ruby).

Pour tous les autres (comme moi), voici de quoi faire dans votre terminal :

{% highlight bash %}
➜  ~  curl -sSL https://get.rvm.io | bash -s stable --ruby
{% endhighlight %}

RVM vous permettra par la suite de switcher entre les versions Ruby très facilement.
Installons une première version de Ruby, la dernière de préférence.

{% highlight bash %}
➜  ~  rvm install ruby
{% endhighlight %}

On peut lister les versions installées avec *rvm ls* :

{% highlight bash %}
➜  ~  rvm ls

rvm rubies

=* ruby-2.1.1 [ x86_64 ]

# => - current
# =* - current && default
#  * - default
{% endhighlight %}

Un petit *ruby -v* vous permet de vérifier quelle version de Ruby est actuellement
utilisée par le système.

{% highlight bash %}
➜  ~  ruby -v
ruby 2.1.1p76 (2014-02-24 revision 45161) [x86_64-darwin12.0]
{% endhighlight %}

En cas de soucis de compatibilité, vous pouvez revenir à la version installée par
Yosemite avec *ruby use system*.

{% highlight bash %}
➜  ~  rvm use system
Now using system ruby.
{% endhighlight %}

{% highlight bash %}
➜  ~  ruby -v
ruby 2.0.0p451 (2014-02-24 revision 45167) [universal.x86_64-darwin13]
{% endhighlight %}

A tout moment vous pourrez utiliser de nouveau la version activée par défaut dans
RVM avec *rvm default*.

Pour ceux qui décourent Ruby, faites donc une petite pause et lancez la commande
*irb*, puis amusez-vous à découvrir Ruby en tapant "Hello World" ou des opérations
mathématiques :)

### Node.js

Node.js est basé sur le [moteur JavaScript de Chrome](http://code.google.com/p/v8/).
Cette plateforme permet notamment de produire très rapidement des applications et
serveurs web en Javascript, avec une petite surcouche de méthodes propres à Node.js.
Sa principale puissance vient du fait qu'il utilisé un système basé sur les évènements et
de surcroît non bloquant (asynchrone) où tout repose sur des callbacks, ce qui le
rend léger et très robuste pour des applications temps-réel qui doivent gérer
un volume de donnée très important.

De nombreux développeurs utilisent désormais Node.js comme base pour réaliser
leurs applications ([Atom](https://atom.io/), [Codebox](https://www.codebox.io/), etc).
Bien que Node.js ne soit pas nécessaire pour utiliser ces applications, il l'est
si vous devez développer vos propres programmes sur ce framework.

Un peu à la manière de RVM, on va utiliser NVM pour gérer nos versions de
Node.js. L'autre avantage (comparé à une installation classique avec Homebrew) c'est
qu'on aura pas à installer NPM (le gestionnaire de paquet pour Node.js) à la main.

{% highlight bash %}
➜  ~  curl https://raw.githubusercontent.com/creationix/nvm/v0.16.1/install.sh | bash
{% endhighlight %}

 Dans le cas où NVM ne renverrait rien dans le Terminal, il se peut que le
script n'ait pas pu ajouter une ligne à vos fichiers de configuration
*~/.bashrc*, *~/.profile*, ou encore *~/.zshrc*. Dans ce cas, il vous suffit
d'ajouter la ligne suivante aux fichiers précédemment cités : *source
~/.nvm/nvm.sh*.

Une fois qu'on a NVM, on en profite pour installer Node.js dans sa dernière
version :

{% highlight bash %}
➜  ~  nvm install 0.10
➜  ~  nvm use 0.10
{% endhighlight %}

L'ensemble des versions de Node.js installées peuvent être visualisées avec *nvm
ls*.

## Quid du Terminal ?

Le Terminal est votre nouveau meilleur ami : si vous ne le connaissez pas
encore, il est grand temps de vous y pencher, mais je ne vais pas vous laissez
seuls face à cette ligne de commande qui fait si peur. D'ailleurs, moi-même je
ne l'utilisais que peu avant de découvrir tous les paquets dont je vais vous
parler dans la suite, et qui transformeront ce vieil ennemi en votre nouveau
partenaire de tous les jours.

> Suivez le guide !

### Exit Bash, bonjour zsh

Dans la famille des [shell Unix](http://fr.wikipedia.org/wiki/Shell_Unix)
(comprendre : interpréteur de commandes), Bash (Bourne-Again shell, jeu de mot
par rapport au Bourne shell, le shell historique d'Unix), est celui installé par
défaut dans OS X. Il n'est pas le meilleur, mais il n'est pas non plus le plus
horrible, il conviendra à tous pour une utilisation ponctuelle. Cependant, si
vous décidez de passer le cap du Terminal, il est fortement recommandé (et je
l'ai adopté) de passer à zsh (pour Z Shell) qui comporte beaucoup
d'améliorations par rapport à Bash, dont : 

* Complétion des commandes, options et arguments.
* Historique des commandes à travers tous les shells zsh actifs.
* etc

On va notamment le coupler à *oh-my-zsh* qui est un framework pour gérer votre
configuration zsh. Commençons donc par installer celui-ci :

{% highlight bash %}
curl -L http://install.ohmyz.sh | sh
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

### Vim

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
curl http://j.mp/spf13-vim3 -L -o - | sh
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
travail et qu'il vous aura inspiré pour améliorer le votre. Désormais, vous
devriez être capable de travailler efficacement dans le terminal et d'avoir de
quoi compiler sous python, ruby et node.js !
