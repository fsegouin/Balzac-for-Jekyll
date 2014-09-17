---
layout: post-no-feature
title: "Préparer Mac OS X Yosemite 10.10 pour la production"
description: "Yosemite arrive, et il est temps de préparer nos machines."
category: articles
comments: true
tags: [mac, osx, unix, yosemite, terminal, vim, ruby, nodejs]
---

![yosemite-macbook]({{ site.url }}/images/yosemite-macbook.png)

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
