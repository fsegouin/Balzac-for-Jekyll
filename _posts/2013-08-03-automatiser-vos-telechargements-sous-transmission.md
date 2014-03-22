---
layout: post-no-feature
title: "Automatiser vos téléchargements sous Transmission"
description: "Avec du RSS et peu d'huile de coude, c'est possible."
category: articles
tags: [bash, internet, programming, unix]
---

Il y a peu, je suis passé de ruTorrent (interface web pour rTorrent) à Transmission suite à des soucis de fiabilité sur ma machine virtuelle. Au départ j’étais un peu perdu, Transmission manque cruellement de fonctionnalités comparé à ruTorrent (modifier le chemin de destination d’un torrent à l’ajout, gestion de flux rss, etc).

Alors forcément les premiers jours ont été difficiles, il a fallu que je m’adapte, et puis finalement, c’est pas si mal que ça. Sans aller sur l’interface web, on peut tout faire (voir même plus) avec Transmission Remote GUI qui est superbe (même s’il lui arrive d’avoir des bugs). On peut enfin modifier le chemin de destination d’un torrent, et pas mal d’autres petites fonctionnalités que l’interface web seule n’offre pas.

Oui mais seulement voilà, il manquait encore quelque chose : la gestion des flux rss. Si comme moi vous suivez plusieurs séries (toutes légales évidemment, cela va de soit) c’est quand même agréable de recevoir les derniers épisodes sans trop se fouler le poignet. On peut très bien installer des packets super sympas comme SickBeard ou CouchPotato pour gérer tout ça, mais ici j’avais envie de le faire à l’ancienne. C’est notamment très utile si vous voulez utiliser les flux rss pour autre chose : de la musique, des ebooks, etc. Trois mots clés ici pour réaliser cette opération : automatic, rss & regex.

## Automatic

Par défaut, Transmission ne gère pas les flux rss (alors que c’est son plus grand défaut depuis déjà pas mal de temps). Il va donc falloir ruser. Certains font leur petit script maison en Python, mais je me suis tourné vers l’excellent [Automatic](http://kylek.is-a-geek.org:31337/Automatic/) développé par [Frank Aurich](https://github.com/1100101). Plusieurs raisons à cela : il gère les notifications [Prowl](http://www.prowlapp.com/) (ce qui vous permettra d’être averti en push sur votre iPhone/iPad d’un torrent ajouté) et intègre une très bonne gestion des filtres sur les flux que vous ajoutez.

### Configuration

Récupérez la dernière version de Automatic depuis le [repo](https://github.com/1100101/Automatic/tags) de l’auteur :

{% highlight bash %}
wget https://github.com/1100101/Automatic/archive/v0.8.3.tar.gz
{% endhighlight %}

On décompacte tout ça où vous voulez (dans /tmp c’est plus sympa) :

{% highlight bash %}
tar -zxvf v0.8.3.tar.gz
cd Automatic-0.8.3
{% endhighlight %}

Avant d’aller plus loin on va installer les dépendances requises (indiquées sur le site de l’auteur & lors de configure) :

{% highlight bash %}
apt-get install libcurl3 libcurl3-dev libxml2 libpcre3 libpcre3-dev
{% endhighlight %}

On passe à l’installation :

{% highlight bash %}
./autogen.sh
./configure.sh
make
make install
{% endhighlight %}

Attention, avant de lancer Automatic, il faut configurer le fichier de configuration, disponible dans /src :

{% highlight bash %}
cd src
vim automatic.conf-sample
{% endhighlight %}

Il va falloir dé-commenter toutes les lignes rpc-* histoire que Automatic puisse dialoguer avec Transmission :

{% highlight bash %}
# For Transmission 1.3x and newer only: set the host on which Transmission runs (default: localhost)
rpc-host = "localhost"
# For Transmission 1.3x and newer only: set the RPC port on which Transmission & Clutch communicate (default: 9091)
rpc-port = 9091
# For Transmission 1.3x and newer only: If you configured Transmission/Clutch to use password authentication, Automatic needs that information as well
rpc-auth = "login:password"
{% endhighlight %}

L’ajout de flux se fait de cette manière. Notez le « id » qui vous permet d’appliquer un ou plusieurs filtres sur des flux différents :

{% highlight bash %}
feed = { url => "http://monflux.rss"
 cookie => ""
 id => 1
 url_pattern => ""
 url_replace => ""
}
{% endhighlight %}

Les filtres s’appliquent sur des flux, différenciés grâce à leur identifiant (feedid). Le pattern à appliquer est une expression régulière (regex). C’est assez intuitif et très puissant, n’hésitez pas à les [tester](http://regexpal.com/) avant l’ajout du filtre!

{% highlight bash %}
filter = { pattern => "Ma Super Serie S.*E.* HDTV x264"
 folder => "/your/download/folder"
 feedid => 1
}
{% endhighlight %}

Enregistrez votre nouveau fichier de configuration avec l’extension .conf cette fois dans /etc (soit /etc/automatic.conf).

Lancez le daemon :

{% highlight bash %}
automatic -o # Permet de ne lancer le programme qu'une fois
automatic # Lance le daemon
{% endhighlight %}

On a désormais Automatic qui tourne en tâche de fond et qui fait tout le travail pour nous. L’autre truc qui serait sympa, c’est d’être averti quand un torrent a été terminé, histoire de vraiment être indépendant de l’interface web. On peut le faire avec Growl (voir plus haut) mais l’application est payante, et j’ai pas trouvé le moyen de garder une trace des notifications push. On va donc tenter par email.

## Notification par email

Depuis peu, Transmission peut lancer des scripts lors de la complétion d’un torrent. On active ça dans le fichier de configuration de Transmission sans oublier de l’arrêter d’abord, sinon le fichier sera écrasé :

{% highlight bash %}
service transmission-daemon stop
vim /etc/transmission-daemon/settings.json
{% endhighlight %}

Modifiez les valeurs des variables suivantes :

{% highlight bash %}
"script-torrent-done-enabled": true,
"script-torrent-done-filename": "/etc/transmission-daemon/complete.sh",
{% endhighlight %}

Voici le script que j’ai écrit (rien de compliqué). N’oubliez pas d’installer les packets bsd-mailx et sendmail-bin ([source](http://blog.nicolargo.com/2011/12/debian-et-les-mails-depuis-la-ligne-de-commande.html)).

{% highlight bash %}
#!/bin/bash

###
### Script to send an email when a torrent finishes
###
 
EMAIL=emailATmailDOTcom # Email address
 
printf "Le torrent $TR_TORRENT_NAME a été téléchargé avec succès à $TR_TIME_LOCATIME\nIl est disponible dans $TR_TORRENT_DIR" | mail -s "Transmission a terminé un téléchargement : $TR_TORRENT_NAME" $EMAIL
{% endhighlight %}

On a plus qu’à redémarrer transmission et le tour est joué :

{% highlight bash %}
service transmission-daemon start
{% endhighlight %}