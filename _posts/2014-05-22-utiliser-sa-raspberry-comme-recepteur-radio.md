---
layout: post-no-feature
title: "Utiliser sa Raspberry Pi comme récepteur radio"
description: "Aussi appelée : radio la plus compliquée au monde. Rien que ça."
category: articles
comments: true
tags: [raspberry, unix, raspbian, tech]
---

![raspberrypi]({{ site.url }}/images/rtl-sdr-rpi.jpg)

Il y a bientôt deux ans, j'avais acheté une clé usb DVB-T (comprendre TNT chez nous) qui à l'époque était très recherchée grâce à ses caractéristiques qui la rendait très particulière.

Elle possède un tuner E4000 d'une plage de fréquence allant de 54 à 2200MHz et un démodulateur FM RTL2832U. Cette excellente combinaison est surtout appréciée du fait que des petits bidouilleurs se sont aperçus que les trames provenant du RTL2832U arrivaient "brutes" au driver. Il n'en fallait pas plus pour que tout le monde détourne la clé pour en faire un récepteur radio large bande à très, très bas coût (environ 20-30€). Ce qui en fait un concurrent plus qu'intéressant face à l'ancien [FUNcube](http://www.funcubedongle.com/) qui était alors la référence.

## Quel intérêt ?

Vu la très grande plage de fréquence qu'on peut obtenir avec le E4000, on peut à peu près faire tout ce qu'on veut pour peu qu'on ait accès à une antenne de qualité. L'utilisation de l'antenne intérieure fournie ne permettra que d'écouter quelques fréquences FM qui émettent assez fort.

Mis à part ça, au-delà du simple récepteur FM, il y a tout un tas de trucs intéressants qu'on peut tirer de cette clé :

* Signal météo (pas essayé)
* Transmissions POCSAG (pagers)
* Signal horaire DCF77 (nécessite une modification du tuner)
* Décodage ADS-B (système de surveillance du contrôle aérien, permet de suivre les avions au-dessus de nos têtes!)
* Ecouter les vigiles des magasins, les conducteurs de bus, etc.

## Installation

Sous Windows, il est plutôt simple d'écouter les ondes avec ce petit dongle :

* On télécharge [SDR#](http://sdrsharp.com/).
* Il suffit de lancer *install.bat* pour qu'il récupère tous les outils dans leurs dernières versions.
* Utilisez *zadig.exe* pour remplacer le driver d'origine par celui de WinUSB.
* Lancez SDR# et sélectionnez RTL-SDR/USB dans la liste des devices.
* Eventuellement, téléchargez un pack de plugins sur [sdrsharpplugins.com](http://www.sdrsharpplugins.com/) qui vous permettra de rajouter une fonction de scanner à SDR#.

Bref sous Windows c'est plus simple. Mais c'est ni portable, ni très pratique. J'ai pas forcément envie de laisser mon pc allumer pour écouter la radio (surtout si on fait du scan...).

> Oui je sais, je pourrai tout aussi bien écouter la radio par 99 autres moyens bien plus simples. Mais c'est ennuyant, non?

Il me fallait donc faire fonctionner cette petite clé sur la Raspberry, cet ordinateur magique de la taille d'une carte de crédit.

## Sur Raspberry Pi

C'est là que les choses deviennent intéressantes :)

On commence par se mettre à jour (pas indispensable si vous l'avez fait il y a peu) :

{% highlight bash %}
➜  ~  sudo apt-get update && sudo apt-get upgrade
{% endhighlight %}

Connectez votre clé TNT à la Raspberry et vérifiez la liste des périphériques connectés :

{% highlight bash %}
➜  ~  lsusb
Bus 001 Device 002: ID 0424:9512 Standard Microsystems Corp.
Bus 001 Device 001: ID 1d6b:0002 Linux Foundation 2.0 root hub
Bus 001 Device 003: ID 0424:ec00 Standard Microsystems Corp.
Bus 001 Device 004: ID 0bda:2832 Realtek Semiconductor Corp. RTL2832U DVB-T
Bus 001 Device 005: ID 0846:9041 NetGear, Inc. WNA1000M 802.11bgn [Realtek RTL8188CUS]
{% endhighlight %}

Si vous obtenez une ligne avec "ID 0bda:2832" c'est gagné, vous pouvez continuer. Pour les autres, votre clé n'est pas compatible, désolé!

On continue par installer de quoi compiler les sources de rtl-sdr :

{% highlight bash %}
➜  ~  sudo apt-get install git cmake libusb-1.0-0.dev build-essential
{% endhighlight %}

On clone le repo de rtl-sdr, on compile et on installe. Attention, l'argument de compilation *-DINSTALL_UDEV_RULES=ON* n'est pas indispensable mais permet de pouvoir lancer rtl-sdr sans être root (pratique sous Raspbian).

{% highlight bash %}
➜  ~  git clone git://git.osmocom.org/rtl-sdr.git
➜  ~  cd rtl-sdr/
➜  ~  mkdir build
➜  ~  cd build
➜  ~  cmake ../ -DINSTALL_UDEV_RULES=ON
➜  ~  make
➜  ~  sudo make install
➜  ~  sudo ldconfig
{% endhighlight %}

![rtl-sdr-1]({{ site.url }}/images/rtl-sdr-1.png)

Un petit reboot plus tard, on lance rtl_test :

{% highlight bash %}
➜  ~  rtl_test
{% endhighlight %}

![rtl-sdr-2]({{ site.url }}/images/rtl-sdr-2.png)

Hmm... Il y a comme quelque chose qui cloche là. Impossible d'ouvrir le device. Heureusement, la solution est plutôt simple. On doit blacklister le chargement automatique de certains modules kernel. Pour cela, on va éditer quelques lignes dans /etc/modprobe.d/raspi-blacklist.conf :

{% highlight bash %}
➜  ~  sudo vim /etc/modprobe.d/raspi-blacklist.conf
{% endhighlight %}

On ajoute les lignes suivantes au fichier :

{% highlight bash %}
blacklist dvb_usb_rtl28xxu
blacklist rtl_2832
blacklist rtl_2830
{% endhighlight %}

Un deuxième reboot plus tard, on relance rtl_test :

![rtl-sdr-3]({{ site.url }}/images/rtl-sdr-3.png)

Tout devrait bien se dérouler en théorie. D'après l'outil de test, si vous n'avez pas de message particulier, c'est ce que c'est bon signe!

On peut désormais écouter un peu de radio FM, j'ai fait un test sur la fréquence 104.2MHz (RTL par chez moi) :

{% highlight bash %}
➜  ~  rtl_fm -f 104.2M -M wbfm -s 400k -r 32k | aplay -r 32k -f S16_LE
{% endhighlight %}

Là, vous devriez vous rendre compte d'un truc... Ca saccade énormément, et on aperçoit ces lignes qui s'accumulent dans le terminal :

{% highlight bash %}
underrun!!! (at least 0.264 ms long)
underrun!!! (at least 0.178 ms long)
underrun!!! (at least 0.092 ms long)
underrun!!! (at least 0.529 ms long)
{% endhighlight %}

Underrun? Hmm, ça sent pas bon. Bon là, je vous avoue, j'ai fait pas mal de recherches sur internet, et je suis tombé sur [ça](https://www.mail-archive.com/osmocom-sdr@lists.osmocom.org/msg00740.html). "Stresser" le CPU permet effectivement de lire le flux de manière correcte. C'est quand même bizarre.

Après avoir creusé un peu plus, il s'avère que c'est en réalité un soucis qui touche toutes les applications utilisants ALSA (Advanced Linux Sound Architecture), le pilote audio utilisé par la Raspberry. Par contre, ça se corrige pas aussi facilement qu'un update de packet, ça veut dire que le soucis est dans le noyau de Linux...

En théorie, avec la dernière version de Raspbian à l'heure où j'écris cet article, vous devriez avoir ce noyau : *Linux raspberrypi 3.10.25+ #622*.
Le [soucis a été réglé](https://www.mail-archive.com/osmocom-sdr@lists.osmocom.org/msg00745.html) depuis la version *3.12.18+ #679* du noyau. Il faut donc mettre à jour le firmware de la Raspberry, en attendant une mise à jour de l'image SD contenant Raspbian proposée sur le site de la [Raspberry Pi Foundation](http://www.raspberrypi.org/).

{% highlight bash %}
➜  ~  sudo rpi-update
{% endhighlight %}

![rtl-sdr-4]({{ site.url }}/images/rtl-sdr-4.png)

Un petit *rpi-update* et un énième reboot plus tard, on tente de nouveau d'écouter notre station...

![rtl-sdr-5]({{ site.url }}/images/rtl-sdr-5.png)

Le son est impec, aucune saccade, bref, on a réussi notre mission !

## Quelques limitations

Comparé à SDR#, rtl-sdr ne peut malheureusement que produire du mono en sortie. C'est pas forcément génant, mais ne vous étonnez pas de ne pas pouvoir activer la stéréo. Autre chose, je n'ai pas vu de support RDS (informations textuelles diffusées en même temps sur la radio), alors que les dernières versions de SDR# le prennent en charge.

## Allons plus loin...

Jusque-là, je vous ai montré comment écouter de la radio FM en bande large (WBFM, radios commerciales). Il est tout aussi possible d'écouter la FM à bande étroite pour écouter n'importe quelle radio portable émettant dans les environs !

> Bonne écoute!
