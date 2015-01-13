---
layout: post-no-feature
title: "Utiliser la virtualisation dans votre workflow"
description: "Ca ne sert pas qu'à tester la dernière distribution Linux"
category: articles
comments: true
tags: [virtualisation, vagrant, puppet, workflow, développement, programmation]
---

![virtualbox-icon]({{ site.url }}/images/virtualbox-icon.png)

La virtualisation est un phénomène qui prend de l'ampleur dans la sphère
informatique. Popularisé dans les années 2000, il a vraiment décollé lorsque les
fabricants de processeurs x86 AMD et Intel ont mis en place la virtualisation
matérielle dans leurs gammes, rendant ainsi son utilisation beaucoup plus
abordable.

C'est aussi ce même processus qui est derrière le récent boom des solutions dans
le cloud telles que Amazon Web Services, Microsoft Azure, ou tout autre provider
de serveurs privés virtuels (VPS). Ils sont tous plus ou moins basés autour de
cette technologie.

## La virtualisation en bref

Sans revenir dans les détails, il existe différentes techniques d'application de
la virtualisation, et l'on parlera dans ce cas de deux types d'hyperviseurs, ces
plate-formes qui permettent la virtualisation.

Alors que l'*hyperviseur de type 1* agit comme un noyau système très léger,
optimisé pour gérer les accès des noyaux d'OS invités à l'architecture
matérielle de l'hôte, l'*hyperviseur de type 2* est lui un logiciel qui tourne
directement sur l'OS hôte, permettant de gérer les différents OS invités. On
peut donc plus parler "d'émulation" de matériel dans ce cas là, les OS invités
croyant alors dialoguer directement avec le matériel.

Si vous avez suivi où je veux en venir, c'est plutôt des solutions d'hyperviseur
de type 2 que nous allons utiliser par la suite. Il existe sur le marché
énormément de solutions, les plus connues étant VMware et VirtualBox, son
équivalent dans le libre.

### Quels avantages à virtualiser ?

Pour faire simple, à la base la virtualisation permet surtout de "cloisonner"
les OS à l'intérieur d'une même machine hôte. Ca permet de faire des essais, de
diviser les processus ou plus simplement de faire tourner différents OS à la
fois.

### Quel rapport avec le développement ?

Tentons de se remettre dans le contexte d'un groupe travaillant sur un même
projet, sans utiliser Vagrant. Le seul moyen de tous travailler sur la même
chose et dans les mêmes conditions, c'est de lister et installer tous les
logiciels dont on a besoin (ex: Apache, MySQL, etc) sur sa machine locale.

Autant ça passait encore quand on codait tous en PHP avec une base de données
MySQL (donc vous avez passé ces dix dernières années dans une grotte
vraisemblablement) mais avec l'avènement des nouvelles applications web écrites
en JavaScript, Python ou encore Ruby, et tout ça avec des dépendances, ça
commence à complexifier la mise en route, surtout si on doit la faire à grande
échelle.

Ca ne s'arrête pas là : les bases de données se sont développées et chacunes a
ses avantages (MySQL, PostegreSQL, Redis, Cassandra, MongoDB, etc.). On rajoute
encore une couche avec les serveurs web qu'on a à notre disposition : entre
Apache, Nginx, Unicorn ou encore Thin, ça fait pas mal de combinaisons à
supporter.

Il est loin le temps du plug-and-play qu'avait essayé d'instaurer LAMP (ou
XAMPP), tout ça devient bien plus compliqué.

### Quel intérêt dans le développement ?

Virtualiser son environnement de développement permet de lancer l'application en
local exactement dans la même configuration que le serveur de production : plus
de surprises, que vous soyez sous Windows, Mac OS X ou Linux.

Puisque votre environnement de développement devient une machîne virtuelle, vous
pouvez en dédier une par projet. Plus de conflits, on peut même partager l'image
de la machine à toute une équipe et le nouveau venu peut se joindre au
développement en moins de temps qu'il n'en faut pour installer une box en une
seule commande avec Vagrant.

* On peut désormais encapsuler son environnement de développement : gain de temps
pour mettre en place son setup & les dépendances.
* Nul besoin d'avoir du matériel dédié : une seule machine hôte pourra héberger
plusieurs OS et donc plusieurs projets au besoin.
* Des versions séparées des bibliothèques et outils de développement. Même si
[rvm](https://rvm.io/) et [nvm](https://github.com/creationix/nvm) ont bien
aidé sur ce plan là, avoir des machines virtuelles dédiées permettent d'éviter
pas mal de soucis, surtout quand on fait des bourdes sur sa machine hôte. Pas
de conflits, pas de soucis.
* Si vous passez du temps à mettre en place une machine virtuelle qui correspond
parfaitement à votre environnement de développement pour un projet spécifique,
il vous suffira de partager son fichier de configuration et de l'envoyer à vos
collaborateurs. Fini les pertes de temps ou les "chez moi ça ne fonctionne pas".
Vous avez tous le même environnement, avec les mêmes versions des outils. Moins
de temps à la mise en place, plus de temps au développement !
* Dupliquer son environnement de développement pour en créer une version de
production. Qui a dit que déployer une application était long et contraignant ?
Un clic. Bam.

> Ok, ça a l'air génial en fait, donc il y a forcément des points négatifs ?

Virtualiser ajoute une couche de complexité dans votre workflow.
Votre setup de projet sera certes plus compliqué au départ, mais vous permettra
de gagner énormément de temps par la suite. Autre aspect à ne pas négliger : les
ressources. Virtualiser duplique en quelque sorte votre OS. C'est comme si vous
tourniez deux systèmes à la fois. Une bonne machine sera nécessaire pour que
tout soit fluide. Certes, une VM est configurée pour être la plus minimale
possible dans notre cas d'utilisation, mais ce n'est pas négligeable non plus.

## Vagrant & VirtualBox

Vagrant est l'outil de notre processus de création de machines virtuelles de
manière rapide et efficace. C'est un ensemble de commandes qui nous permettra de
monter, d'après un simple fichier de configuration, toute la machine virtuelle.
Il se base sur l'hyperviseur VirtualBox, qui est libre, et maintenu par Oracle.
Il peut aussi être doté de plugin pour supporter VMware, dans une version
payante.

Installons donc tout ça :

* [VirtualBox](https://www.virtualbox.org/wiki/Downloads) sans oublier le
VirtualBox Oracle VM VirtualBox Extension Pack sur la même page
* [Vagrant](https://www.vagrantup.com/downloads.html)

Vérifions que Vagrant est bien installé et accessible depuis votre path :

{% highlight bash %}
➜  ~ vagrant --version
Vagrant 1.7.2
{% endhighlight %}

### Initialiser des box avec Vagrant

Le but de Vagrant va être de nous proposer des "box" un peu comme des images de
machines virtuelles pré-configurées que l'on pourra utiliser très rapidement.
Auparavant, monter une machine virtuelle nécessitait d'avoir un fichier image
d'un CD d'installation, d'installer l'OS, etc. Puis configurer l'environnement. Bref
tout ça était long et contraignant.

Allons donc sur le catalogue de box
[Atlas](https://atlas.hashicorp.com/boxes/search) pour obtenir une liste de box
que l'on peut essayer dès maintenant. Bien sûr, dans un futur proche, on pourra
créer nos propres boxes avec notre environnement optimisé aux petits oignons,
mais il faut bien commencer avec les bases.

Chaque box peut contenir soit un OS en tant que base, mais aussi des
environnements déjà installés comme des stacks LAMP pour développer en PHP, ou
Ruby, Python, etc.

Installons une box dans notre collection. Une des box les plus populaires, c'est
celle de Hashicorp, nommée Precise64. Elle se base sur un Ubuntu 12.04 LTS
64-bit. Ce n'est pas la dernière version de Ubuntu comme vous l'aurez remarqué,
mais une version LTS (Long Term Support) qui est préférable si on compte mettre
en production.

{% highlight bash %}
➜  ~ vagrant box add hashicorp/precise64
==> box: Loading metadata for box 'hashicorp/precise64'
box: URL: https://atlas.hashicorp.com/hashicorp/precise64
This box can work with multiple providers! The providers that it
can work with are listed below. Please review the list and choose
the provider you will be working with.

1) hyperv
2) virtualbox
3) vmware_fusion

Enter your choice: 2
==> box: Adding box 'hashicorp/precise64' (v1.1.0) for provider: virtualbox
box: Downloading: https://atlas.hashicorp.com/hashicorp/boxes/precise64/versions/1.1.0/providers/virtualbox.box
{% endhighlight %}

Suivant l'hyperviseur (provider dans le terme de Vagrant) que vous utilisez,
sélectionnez celui sur lequel vous comptez utiliser la box.

Envie de développer avec le framework Laravel ? Aucun soucis, il y a une box
déjà configurée pour ça !

{% highlight bash %}
➜  ~ vagrant box add laravel/homestead
{% endhighlight %}

Quel que soit votre environnement souhaité ou la stack sur laquelle vous
travaillez, la plupart des solutions "de base" sont déjà disponibles, vous
permettant d'être productif très rapidement.

Vérifions que notre box precise64 est bien dans notre collection :

{% highlight bash %}
➜  ~ vagrant box list
precise64 (virtualbox)
{% endhighlight %}

Tout va bien. Créons un dossier et initialisons (création d'un Vagrantfile) la
machine virtuelle à partir de cette box.

{% highlight bash %}
➜  ~ mkdir myVM
➜  ~ cd myVM
➜  ~/myVM vagrant init hashicorp/precise64
A `Vagrantfile` has been placed in this directory. You are now
ready to `vagrant up` your first virtual environment! Please read
the comments in the Vagrantfile as well as documentation on
`vagrantup.com` for more information on using Vagrant.
{% endhighlight %}

> Avant d'aller plus loin, veuillez à ne pas lancer des box depuis des chemins
qui contiennent des espaces. On est en 2015, mais visiblement Vagrant n'est pas
trop fan.

{% highlight bash %}
➜  ~/myVM vagrant up
Bringing machine 'default' up with 'virtualbox' provider...
==> default: Importing base box 'hashicorp/precise64'...
==> default: Matching MAC address for NAT networking...
==> default: Checking if box 'hashicorp/precise64' is up to date...
==> default: Setting the name of the VM: precise64-test_default_1421172526586_93193
==> default: Clearing any previously set network interfaces...
==> default: Preparing network interfaces based on configuration...
default: Adapter 1: nat
==> default: Forwarding ports...
default: 22 => 2222 (adapter 1)
==> default: Booting VM...
==> default: Waiting for machine to boot. This may take a few minutes...
default: SSH address: 127.0.0.1:2222
default: SSH username: vagrant
default: SSH auth method: private key
default:
default: Vagrant insecure key detected. Vagrant will automatically replace
default: this with a newly generated keypair for better security.
default:
default: Inserting generated public key within guest...
default: Removing insecure key from the guest if its present...
default: Key inserted! Disconnecting and reconnecting using new SSH key...
==> default: Machine booted and ready!
==> default: Checking for guest additions in VM...
default: The guest additions on this VM do not match the installed version of
default: VirtualBox! In most cases this is fine, but in rare cases it can
default: prevent things such as shared folders from working properly. If you see
default: shared folder errors, please make sure the guest additions within the
default: virtual machine match the version of VirtualBox you have installed on
default: your host and reload your VM.
default:
default: Guest Additions Version: 4.2.0
default: VirtualBox Version: 4.3
==> default: Mounting shared folders...
default: /vagrant => /Users/you/myVM
{% endhighlight %}

Plusieurs choses ici : vagrant vient de lancer la machine virtuelle, désormais
accessible par SSH. Le dossier partagé /vagrant est relié au même dossier où
vous avez initialisé la machine virtuelle, permettant de partager n'importe quel
fichier si nécessaire.

{% highlight bash %}
➜  ~/myVM vagrant ssh
Welcome to Ubuntu 12.04 LTS (GNU/Linux 3.2.0-23-generic x86_64)

* Documentation:  https://help.ubuntu.com/
New release '14.04.1 LTS' available.
Run 'do-release-upgrade' to upgrade to it.

Welcome to your Vagrant-built virtual machine.
Last login: Fri Sep 14 06:23:18 2012 from 10.0.2.2
vagrant@precise64:~$
{% endhighlight %}

Pour ce qui est de suspendre, reprendre ou supprimer la machine virtuelle, les
commandes *vagrant suspend*, *vagrant resume* et *vagrant destroy* devrait vous
aiguiller.

## Créer des box avec Vagrant

On sait désormais télécharger des "box" qui serviront de base. Il est temps de
créer de box qui permettront de développer nos projets.

### Le Vagrantfile

C'est dans le *Vagrantfile* dont j'ai parlé plus haut que nous allons pouvoir
customiser notre box pour chaque projet. Chaque projet a un unique Vagrantfile.

Chaque machine virtuelle est créée à partir de ce fichier de configuration. Ici,
on en créé un vierge qui prend pour base la box *precise64*. C'est dans ce même
fichier qu'on pourra configurer quelques aspects propres à cette box :

* Permettre ou non à la box de se mettre à jour à chaque lancement
* Forwarder des ports entre l'hôte et la machine virtuelle
* Partager un dossier avec la machine hôte
* Définir un script de provisioning : c'est cet aspect qui sera le plus
intéressant pour personnaliser vos box

Le plus intéressant est bien de placer ce Vagrantfile sous version control.
Chaque nouveau membre va récupérer le Vagrantfile nécessaire pour créer la
machine virtuelle.

Créons un nouveau dossier pour cette machine virtuelle et on initialise un
Vagrantfile avec pour base la box *precise64* :

{% highlight bash %}
➜  ~ mkdir new-box
➜  ~/new-box vagrant init hashicorp/precise64
{% endhighlight %}

### Provisioners

On appelle provisioners les outils qui nous permettront d'automatiser la mise en
place des outils nécessaires à l'environnement de développement que l'on
souhaite créer. Vagrant supporte bien évidemment des scripts shell, mais
également des outils tels que Chef ou Puppet.

#### Avec un script shell

C'est le plus simple et le plus basique, il suffit de donner un script shell
dans la partie appropriée de notre Vagrantfile et le script sera exécuté à
l'initialisation de la machine virtuelle.

Imaginons que l'on souhaite automatiser l'installation d'Apache :

{% highlight bash %}
#!/usr/bin/env bash
echo "Installation d'Apache..."
apt-get update >/dev/null 2>&1
apt-get install -y apache2 >/dev/null 2>&1
rm -rf /var/www
ln -fs /vagrant /var/www
{% endhighlight %}

On sauvegarde ce script dans un fichier provision.sh et on ajoute les lignes
suivantes dans le Vagrantfile :

{% highlight bash %}
config.vm.provision: shell, path: './provision.sh'
config.vm.forward_port 80, 8080
{% endhighlight %}

On vient de demander à ce que le script que l'on a écrit soit lancé et par la
même occasion forwarder le port 80 de la machine virtuelle sur le port 8080 de
notre machine hôte. De la sorte, on pourra tester sur l'hôte ce que le serveur
Apache nous renvoie.

#### Avec Puppet

Cette fois, on demande à Vagrant d'utiliser Puppet, un superbe outil de
provisioning. Sous une syntaxe plutôt agréable à utiliser, on pourra automatiser
tout un tas de commandes comme la création d'utilisateurs, de liens, de scripts à
lancer, packages à installer ou autre.
Dans le Vagrantfile :

{% highlight bash %}
config.vm.provision :puppet
{% endhighlight %}

Créons ensuite un manifest *default.pp* dans le dossier *manifests* :

{% highlight ruby %}
package { 'apache2':
  ensure => 'installed',
}

file { 'site-config':
  path => "/etc/apache2/sites-enabled/000-default",
  source => "/vagrant/site-config",
  require => Package["apache2"],
}

service { 'apache2':
  ensure => 'running',
  hasrestart => true,
  subscribe => File["site-config"],
}

file { '/vagrant/index.html':
  content => "<h1>Vagrant & Puppet, le combo parfait</h1>",
}
{% endhighlight %}

Pour le fichier *site-config*, prenez n'importe quel fichier de défaut pour
Apache, ça devrait le faire. Remplacez juste le DocumentRoot par */vagrant*
histoire de pouvoir synchroniser le site.

N'oubliez pas que le dossier */vagrant* sur la machine virtuelle est notre
dossier synchronisé avec la machine hôte. De la sorte, le fichier site-config
peut être configuré à l'extérieur de la machine virtuelle très facilement.

Je vois renvoie vers la
[doc](https://docs.puppetlabs.com/puppet_core_types_cheatsheet.pdf) de Puppet si
vous voulez plus d'infos pour créer vos fichiers de provisioning.

La bonne nouvelle ici, c'est qu'on a même plus besoin d'utiliser SSH pour
intéragir avec le serveur. Toutes vos modifications dans le default.pp pourront
être "pushées" grâce à la commande *vagrant provision* :

{% highlight bash %}
➜  ~/new-box vagrant provision
{% endhighlight %}

Besoin de modifier à nouveau le fichier index.html ? Bonne nouvelle, il est dans
notre dossier synchronisé ! Un coup de vim sur le fichier html, on sauvegarde,
et le serveur Apache sur la machine virtuelle a lui aussi la nouvelle version.

Comme n'importe quel langage de programmation, Puppet peut encapsuler votre code
dans des classes et méthodes. Créant ainsi de véritables modules très complets
comprenant plusieurs manifests.

#### Le puppet forge

On peut aussi utiliser le [puppet forge](https://forge.puppetlabs.com), un
repository avec un grand nombre de modules puppet pour automatiser tout ça (et
aussi gagner du temps).

Par exemple, on pourrait refaire tout ce qu'on a fait en utilisant un module
nommé *puppetlabs/apache*.

Il vous faut d'abord installer le package
[puppet](https://docs.puppetlabs.com/guides/install_puppet/install_osx.html) sur
votre machine hôte.

{% highlight bash %}
➜  ~ puppet module install puppetlabs/apache
{% endhighlight %}

Afin de donner à notre Vagrantfile le chemin du nouveau module installé, il
suffit d'ajouter les lignes suivantes :

{% highlight ruby %}
config.vm.provision :puppet do |puppet|
  puppet.module_path = "/Users/you/.puppet/modules"
end
{% endhighlight %}

Le module installé va nous permettre d'écrire un manifest qui pourra utiliser
les méthodes ce module :

{% highlight ruby %}
include apache

Exec {
  path => ["/usr/bin", "/usr/local/bin"],
}

exec { "update":
  command => "apt-get update",
}

apache::vhost { "personal-site":
  port => 80,
  docroot => "/vagrant",
}

file { "/vagrant/index.html":
  content => "<h1>Vagrant, Puppet et les modules : un triptyque du tonnerre</h1>",
}
{% endhighlight %}

En quelques lignes et grâce aux méthodes du module *puppetlabs/apache*, on peut
désormais s'assurer qu'Apache soit installé et le configurer sans même toucher
à un fichier de la machine virtuelle.

Besoin d'installer et utiliser [wget](https://forge.puppetlabs.com/maestrodev/wget)
dans votre script d'installation ? Aucun soucis.

{% highlight bash %}
➜  ~ puppet module install maestrodev-wget
{% endhighlight %}

Dans votre manifest :

{% highlight ruby %}
include wget

wget::fetch { 'http://www.google.com/index.html':
  destination => '/tmp/index.html',
  timeout     => 0,
  verbose     => false,
}

{% endhighlight %}

## Aller plus loin

Un même Vagrantfile peut configurer plusieurs machines virtuelles (cf. [doc
Vagrant](https://docs.vagrantup.com/v2/multi-machine/index.html)). Imaginez un
seul fichier de configuration qui setup votre serveur, votre base de données et
peut être même un client qui s'y connecte ? Libre à vous de vous plonger dans la
doc :)

Puppet est un outil super flexible avec son système de modules et très, très
performant lorsqu'il est bien utilisé. Seulement voilà, comme tout nouvel outil,
il demande un peu de travail avant de pouvoir être utilisé efficacement. Comme
je suis sympa et que j'aime les cookbooks, voilà de quoi vous mettre sur les
[rails](http://puppetcookbook.com/) !
