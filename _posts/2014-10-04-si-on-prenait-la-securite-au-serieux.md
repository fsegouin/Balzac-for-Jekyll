---
layout: post-no-feature
title: "Si on prenait la sécurité au sérieux ?"
description: "Comment les dernières mesures d'Apple pourraient enfin faire changer
les choses."
category: articles
comments: true
tags: [mac, osx, unix, apple, sécurité, filevault, 2fa, icloud]
---

![timer-ptr]({{ site.url }}/images/TouchID.png)

> Après plusieurs annonces visants à améliorer la sécurité de ses utilisateurs,
Apple semble enfin comprendre qu'il est temps de faire changer les mentalités.

Début Septembre, le
"[CelebGate](http://blog.kaspersky.fr/le-sombre-avenir-du-cloud/3645/)", bien
relayé par les médias pour son aspect people, a fait couler beaucoup d'encre.
Véritable prouesse de reverse engineering ou simple
[bruteforce](https://github.com/hackappcom/ibrute) exploitant une faille peu
regardante sur le nombre de tentatives à la connexion ?

Le plus grave dans cette histoire n'est pas tellement que des hackeurs
cheuvronnés ont réussi à utiliser une faille à des fins peu orthodoxes, mais
plutôt qu'une dure réalité vient de toucher à peu près tous ceux qui se
pensaient jusque-là protégés avec leurs données : il est désormais impossible de
savoir quel type de données ni même où nos données sont sauvegardées dans le
cloud, que cela soit celui d'Apple ou un autre.

Pour autant, est-il nécessaire d'avoir peur du cloud ? Oui et non. Nous sommes
désormais entrés dans une nouvelle ère numérique où nos données circulent sans
même que nous en prenions conscience. Le problème est autant dans la nature de
celles-ci que dans leur destination. Les plus réfractaires diront qu'ils
refusent de stocker quoi que ce soit dans le cloud (avec en plus une belle peur
du [Patriot Act](http://fr.wikipedia.org/wiki/USA_PATRIOT_Act)) et d'autres
diront qu'ils n'en ont rien à faire tant que leurs données restent accessibles
où qu'ils soient (et c'est quand même bien là le but). Seulement voilà : laisser
nos données dans le nuage c'est aussi accepter de ne plus garder la main mise
sur celles-ci, d'où le début d'une question de confiance sur le prestataire de
service. Apple, Dropbox, Microsoft, elles proposent toutes le même service mais
pas forcément les mêmes moyens pour s'assurer que nos données soient gardées à
l'abris des regards indiscrets.

> Pourquoi iCloud plus qu'un autre ?

J'ai décidé d'orienter cet article sur les produits de l'écosystème Apple pour
une bonne raison : c'est lui qui est désormais la cible préférée de nos petits
pirates. Tout d'abord car il faut se rendre à l'évidence, Apple s'impose de plus
en plus dans tous les foyers, que cela soit par les Mac ou les appareils mobiles
iPhone/iPad, mais surtout que ses clients utilisent bien souvent plusieurs
appareils au sein de cet écosystème bien fermé. En utilisant iCloud, on s'assure
de bénéficier de toutes nos données sur chacun de nos appareils, et ce, à tout
moment. Sur le papier, c'est quand même une technologie qui fait rêver. Mais
elle a aussi un gros point faible : tout est centralisé sur votre compte. Une
fois son accès obtenu, c'est votre vie numérique qui se retrouve en danger.

> Que faire pour se protéger sans couper le cordon ?

Il semble impossible aujourd'hui de totalement vouloir (et pouvoir) couper le
cordon du cloud pour se protéger. De toute façon, vos données circulent contre
votre gré par bien d'autres moyens. Il faut plutôt penser intelligement et
savoir utiliser des systèmes déjà en production mais peu connus pour mieux se
protéger.

Ce n'est pas tellement le résultat d'une faille depuis bouchée qui a
marqué le renouveau de la sécurité chez Apple, mais plutôt une volonté d'enfin
utiliser des technologies arrivées à maturité, et qu'il est temps de proposer
(et d'imposer) au grand public.

## Se protéger efficacement

### FileVault : le chiffrement à la portée de tous

![timer-ptr]({{ site.url }}/images/filevault.png)

Il y a une dizaine d'années, chiffrer ses données pour un particulier relevait
plutôt de l'extravagance que de la réelle utilité. D'ailleurs, aucun système
d'exploitation ne proposait jusque là une solution clé en main pour le faire.
Chiffrer ses données (et son disque principal) signifiait alors des pertes de
performances non négligeables et quand on sait que le temps d'accès aux données
n'est jamais trop court, c'est un chemin que peu se risquaient à prendre. Mais
depuis l'apparition de [FileVault 2](http://support.apple.com/kb/HT4790?viewlocale=fr_FR) sur Lion,
Apple propose un outil de chiffrement très efficace... A tel point qu'elle vient
de le rendre actif par défaut depuis la publication de la Golden Master de Yosemite,
ce qui laisse donc à présager que cela sera également le cas sur la version finale.

Véritable volonté de la part d'Apple, elle pousse désormais tous ses clients
à chiffrer leurs données. Quand on sait que la plupart de ces derniers ne changent
jamais la configuration par défaut, c'est un grand pas en avant que fait Apple
dans la sécurisation des données de ces derniers. Chapeau bas !

### TouchID

Bien qu'on ai tous en tête les prouesses de James Bond pour déjouer les lecteurs
d'empruntes digitales, ils restent encore à l'heure actuelle un système
d'authentification bien plus sûr que les traditionnels mots de passe. Son
apparition sur l'iPhone 5S et sa généralisation très prochainement sur la gamme
des iPad Late 2014 laisse présager qu'Apple a envie que le bon vieux code à 4
chiffres prenne sa retraite. Avec en ligne de mire Apple Pay, la sécurité de
votre téléphone (et donc dans un futur proche vos moyens de paiement) se doit
d'adopter un système bien plus sécurisé qu'un code à 4 chiffres qui continue
pourtant d'être utilisé sur nos cartes à puces traditionnelles. Alors qu'IBM le
faisait déjà au début des années 2000, Apple n'a pas encore laissé présager
l'utilisation d'un lecteur TouchID sur ses portables, cela serait pourtant un
atout plutôt agréable...

Autre défi de taille, c'est la sécurité des données médicales qui commencent à
faire leur apparition dans de nombreux smartphones et objets connectés. De plus
en plus de personnes ont des bracelets ou trackers d'activités qui recueillent
constamment un volume de donnée important qui tend à être centralisé. Avec
Medical ID, cette fonctionnalité qui permettrait à quiconque de consulter votre
dossier médical très rapidement depuis votre téléphone, même verrouillé, si
jamais il vous arrivait quelque chose, Apple propose ici une très bonne idée que
l'on devrait tous adopter. Il serait alors très simple de vérifier une allergie à
un produit spécifique ou votre groupe sanguin par exemple. Autant de données
très utiles mais qui peuvent se révéler aussi plutôt sensibles. Apple se doit
de généraliser l'usage de TouchID à ce type de données.

### L'authentification à deux étapes

On parle d'authentification "forte" (ou "multi-facteur") lorsque cette dernière
réunit au moins deux des trois facteurs d'authentification indépendants que sont :

* La connaissance : quelque chose que seul l'utilisateur connaît (mot de passe)
* La possession : quelque chose que seul l'utilisateur détient (carte à puce,
  clé, token, smartphone, etc)
* L'inhérence : quelque chose d'unique lié à l'utilisateur (biométrie)

Suite au CelebGate, Apple a voulu renforcer drastiquement ses mesures de
sécurité visant iCloud, et cela passait notamment par l'authentification en deux
étapes. Ce système visant à ajouter au traditionnel mot de passe un canal de
communication sécurisé (ici, en suivant notre modèle multi-facteur, la
possession), vient vous communiquer un second mot de passe (souvent une série de
chiffres) qui vous est envoyé par sms ou directement généré par une application
dédiée. L'idée derrière cela c'est de vérifier l'authenticité de celui qui tente
de se connecter : en utilisant un canal plus sécurisé et direct tel que votre
téléphone ou un jeton de sécurité (qu'il soit hardware ou software), on s'assure
que la personne qui en fait la demande est bien celle qui est autorisé à le
faire. D'où le nom d'authentification à deux étapes : d'abord la connaissance
(votre mot de passe), puis la possession (second code envoyé sur votre
téléphone).

Ce système était déjà disponible mais pas étendu à tous les services Apple, ce
qui est désormais le cas. Cependant, Apple doit ici franchir une seconde étape :
la rendre obligatoire par défaut. A aucun moment sur leur site, cette fonction
n'est clairement indiquée comme disponible. Trop peu de personnes savent ce
qu'est l'authentification en deux étapes et elle est pourtant indispensable. A
une heure où votre mot de passe n'est plus la clé inviolable qu'elle était il y
a encore peu, le canal pour vous joindre, lui, l'est beaucoup plus. Votre mobile
reste avec vous, et devrait être le seul moyen pour vérifier votre authenticité.
Certes c'est une étape supplémentaire à la connexion que beaucoup vont dénigrer,
mais quand il est question de sécurité des données, cette étape deviendra
bientôt indispensable.

## La sécurité ne passe pas que du côté du client...

... et Apple l'a bien compris. En liant des appareils à nos comptes, on les
autorise de facto à accéder à toutes nos données sur le cloud et donc récupérer
absolument tout ce qui y est échangé. Mais que faire si notre appareil venait à
être cloné ? Après tout, il suffirait de "dupliquer" l'identité d'un appareil
pour faire croire au système qu'il est authentique, et donc bénéficier d'une
autorisation au préalablement donnée.

C'est là qu'Apple à depuis peu mis en place une autre couche de sécurité sur ses
services en ligne. iMessage dispose depuis cet été d'un système de sécurité
plutôt étonnant et documenté quasiment nulle part, qui sera sûrement très
prochainement étendu à d'autres services. En effet, Apple a décidé de jouer sur
le fait qu'elle ne fait pas que mettre à disposition un service de données dans
le nuage, mais qu'elle est avant maîtresse de ses machines. Chaque Mac dispose
(en dur) des numéros de série qui sont stockés dans une NVRAM sur la carte mère.
Cette mémoire vive non-volative garde les données de façon persistante, comme
son nom l'indique. Apple s'en sert pour stocker des valeurs liées à la machine
et qui ont pour particularité d'être uniques. Couplées au numéro de série du
Mac, elles servent à générer un couple clé privée/clé publique qu'Apple
utilisera pour vous authentifier. A chaque connexion, Apple vérifie donc la
valeur de ces données et valide les clés. Une seule inconsistance dans une des
valeurs données rendra la clé non utilisable et lèvera une alerte sur le compte
iCloud utilisé lors de la connexion, rendant la machine clonée inutilisable.

Apple fait cela pour plusieurs raisons : d'abord, dans un premier temps, pour
lutter contre le spam qui commence à sévir sur iMessage. Ensuite, pour commencer à
rendre son service un peu plus difficile à contourner pour ceux qui
souhaiteraient s'approprier des comptes iCloud.

## Conclusion

Alors qu'Apple commence à proposer (et imposer) à ses utilisateurs des moyens
de sécurité avancés, il serait bien que la concurrence marche dans les pas du
géant californien et fasse de même. ["L'Internet of Things"](http://fr.wikipedia.org/wiki/Internet_des_objets) qui caractérise si
bien l'évolution que connaît à l'heure actuelle Internet se doit de se doter de
moyens de sécurité qui suivent son évolution. Plus nos données sont dans le
cloud et plus notre intégrité digitale se retrouve menacée. Les moyens
d'authentification ont aussi évolué, mais c'est surtout maintenant au tour des
sociétés de sensibiliser leurs clients sur ces nouvelles pratiques à adopter.  
