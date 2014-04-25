---
layout: post-no-feature
title: "A l'aventure du Mac Pro"
description: "Un Mac Pro pour trois fois moins cher, c'est possible?"
category: articles
comments: true
tags: [apple, osx, mac, tech]
---

![htop]({{ site.url }}/images/mac-pro-1.png)

S'il y a bien un domaine dans lequel j'ai très vite plongé il y a quelques années, c'est celui de la bidouille. Alors forcément quand on a entendu en 2006 que les Mac allaient passer sur des CPU Intel, je me suis dit que ça allait être plutôt amusant de faire tourner OS X sur nos PC.

On entend bien souvent parler du terme "Hackintosh" pour désigner ces PC qui font tourner OS X. Ca peut sembler un peu barbare, voir même plutôt repoussant, mais il y a un fond de vérité à tout cela. Car s'il y a le terme "hack" dans cette désignation, c'est pas pour rien. Même si OS X est désormais un OS entièrement compatible avec nos plateformes x86, cela n'en reste pas moins un exercice périlleux... mais pas impossible. A vrai dire, avec les années, tout cela s'est véritablement simplifié et monter un "Hackintosh" en 2014 et bien plus aisé qu'il y a quelques années de cela.

## Tout est question de compatibilité

Avec les progrès des installeurs que l'on trouve sur Internet, réaliser une clé usb bootable permettant d'installer Mac OS X est un jeu d'enfant. Il existe même (dans certains recoins peu éclairés du web) des images prêtes à être grillées sur des clé usb, même plus besoin d'avoir un Mac à la base!

La référence dans ce domaine à l'heure actuelle, c'est l'excellent [tonymacx86](http://www.tonymacx86.com/). Non seulement il publie chaque mois des configurations "types" censées être compatibles sans le moindre effort, mais en plus il participe activement sur la scène des "Hackintosh" en publiant ses propres outils pour installer l'OS et autres pilotes nécessaires. Il est aussi intéressant de garder le site sous le coude en cas d'update de l'OS : il sera le premier à vous dire quelle est la démarche à effectuer.

## Réaliser son propre Mac Pro

Dans ma quête d'un Mac surpuissant, j'ai toujours voulu obtenir un Mac Pro. D'ailleurs, les Mac Pro 2013 sont tout autant des bijoux de technologie que de design. Cette forme cylindrique leur confère cet aspect "objet de mode" auquel très peu de PC peuvent prétendre. Apple a toujours eu cette petite touche supplémentaire : le design au service de la puissance.

Seulement voilà, petit bémol, les tarifs des Mac Pro 2013 commencent à 2.999,00 € dans leur configuration la plus basse :

* Processeur Intel Xeon E5 quadricœur à 3,7 GHz
* 12 Go de mémoire ECC DDR3 à 1 866 MHz
* Deux AMD FirePro D300 avec 2 Go de VRAM GDDR5 chacun
* 256 Go de stockage flash PCIe1

*Outch!*

Beaucoup diront qu'évidemment à ce prix, on paye l'objet. C'est vrai qu'il est magnifique et plutôt atypique, et que très certainement si j'en avais les moyens, je l'aurai acheté sans trop de remords. La majorité d'entre nous cependant, n'auront pas le porte-monnaie nécesaire pour débourser autant dans un Mac survitaminé. Il est également dit qu'un iMac en configuration haut de gamme n'arrive même pas à la cheville du plus petit des Mac Pro, ou alors seulement en benchmark. A l'usage, c'est le jour ou la nuit.

Pour ce Mac Pro "homemade", on va essayer de se rapprocher le plus possible de la configuration du Mac Pro d'entrée de gamme. On doit cependant faire quelques concessions : 

* **CPU** : un Intel Xeon E5 1620v2 coûte à l'heure actuelle environ 300€. Le haut de gamme dans la série des i7 pourra facilement prétendre à être un bon équivalent. Cependant, ils valent quasiment le même prix et si on veut rogner un peu pour passer la barre des 1000€, il va falloir se rabattre sur les i5. Un bon équivalent dans la gamme des i5 sera probablement le i5-4670k à environ 200€. Attention, il s'agit bien du modèle "k", avec coefficient débloqué. J'expliquerai un peu plus tard pourquoi.
* **GPU** : les deux AMD FirePro D300 correspondent plus ou moins à des AMD FirePro W7000 modifiées. A plus de 750€ la carte, on réalise tout de suite que 50% du prix de la machine passe en réalité dans son **énorme** GPU. Encore une fois, les pros qui ont véritablement besoin d'une telle puissance savent qu'ils faut y mettre le prix et n'hésiteront pas à le faire. Les plus novices d'entre nous se contenteront donc d'une bonne carte graphique "de bureau" pour leurs travaux occasionnels.
* **Thunderbolt** : les cartes mères avec un support Thunderbolt restent encore rares, et plutôt chères. De plus, elles seront assez difficiles à faire fonctionner nativement. Il est donc important de prendre en considération que ce Mac Pro "homemade" n'aura pas de support Thunderbolt. Cela peut être embetant pour les pros qui ont absolument besoin de cette nouvelle connectique. Les plus aventureux d'entre eux trouveront des tutoriaux sur le net, les autres auront déjà acheté un Mac Pro et ne liront pas ce genre de billet... ;)

## Prêts? On passe à la caisse!

Type|Item|Price
:----|:----|:----
**CPU** | [Intel Core i5-4670K 3.4GHz Quad-Core Processor](http://uk.pcpartpicker.com/part/intel-cpu-bx80646i54670k) | £161.99 @ Aria PC 
**CPU Cooler** | [Corsair H60 54.0 CFM Liquid CPU Cooler](http://uk.pcpartpicker.com/part/corsair-cpu-cooler-h60cw9060007ww) | £49.98 @ Ebuyer 
**Motherboard** | [Gigabyte GA-Z87MX-D3H Micro ATX LGA1150 Motherboard](http://uk.pcpartpicker.com/part/gigabyte-motherboard-gaz87mxd3h) | £98.24 @ Scan.co.uk 
**Memory** | [G.Skill TridentX 16GB (2 x 8GB) DDR3-1866 Memory](http://uk.pcpartpicker.com/part/gskill-memory-f31866c8d16gtx) | £143.60 @ Amazon UK 
**Storage** | [Samsung 840 Pro Series 256GB 2.5" Solid State Disk](http://uk.pcpartpicker.com/part/samsung-internal-hard-drive-mz7pd256bw) | £137.49 @ Amazon UK 
**Video Card** | [Gigabyte GeForce GTX 750 Ti 2GB Video Card](http://uk.pcpartpicker.com/part/gigabyte-video-card-gvn75toc2gi) | £113.70 @ Scan.co.uk 
**Case** | [BitFenix Prodigy M Arctic White MicroATX Mini Tower Case](http://uk.pcpartpicker.com/part/bitfenix-case-bfcprm300wwxkwrp) | £65.52 @ Aria PC 
**Power Supply** | [Corsair CX 750W 80+ Bronze Certified Semi-Modular ATX Power Supply](http://uk.pcpartpicker.com/part/corsair-power-supply-cx750m) | £71.92 @ Aria PC 
**Wireless Network Adapter** | [TP-Link TL-WDN4800 802.11a/b/g/n PCI-Express x1 Wi-Fi Adapter](http://uk.pcpartpicker.com/part/tp-link-wireless-network-card-tlwdn4800) | £27.97 @ Aria PC 
 | | **Total** 
 | [PCPartPicker part list](http://uk.pcpartpicker.com/p/3mITn)  | £870.41 (1051.68€)

En partie basée sur le **CustoMac Pro** de [tonymacx86](http://www.tonymacx86.com/416-building-customac-buyer-s-guide-march-2014.html#custo_pro), cette configuration permet de diviser par trois le prix d'un Mac Pro 2013 !

### GPU

Pour les plus attentifs d'entre vous, vous remarquerez que la GeForce GTX 750 Ti qui vient de sortir il y a peu, n'est pas encore supportée sous Mac OS X. Elle utilise une nouvelle puce (Maxwell) qui n'a pas encore trouvé sa place dans les Mac. Il est dit qu'Apple va mettre à jour sa gamme afin de les introduire. Son support "out-of-the-box" devrait donc se faire sous peu. En attendant, il est toujours possible de booter sur le GPU intégré, le HD4600, qui est totalement supporté et suffira à nous faire patienter pour ce futur pilote :)

### SSD

Les SSD deviennent un standard en 2014 : exit le bon vieux HDD et place aux performances ! Un 840 Pro Series 256GB vous permettra d'obtenir 500MB/s aussi bien en écriture qu'en lecture, de quoi faire aussi bien voir même mieux que ceux fournis dans les Mac Pro.

### Refroidissement

Les Mac Pro 2013 sont plébiscités pour leur silence. Pourquoi ne pas tenter la voie du watercooling? Le Mac Pro n'en utilise évidemment pas, puisqu'il n'est composé que d'un seul ventilateur central, mais c'est le mieux à faire pour avoir du silence sur nos PC. Un Corsair H60, H80 ou H100i (radiateur double) pour les plus gros boîtiers permettra de s'en rapprocher.

### Boitier

![mac-pro-bitfenix-prodigy]({{ site.url }}/images/mac-pro-bitfenix-prodigy.jpg)

Côté boîtier, je suis tombé amoureux de ce Bitfenix Prodigy ! Disponible en version mini-ATX et Micro-ATX, son petit look mini "Mac Pro d'ancienne génération" est très apprécié dans la communauté des Hackintosheurs. Autre point positif : il permet de recevoir des cartes graphiques plutôt longues et s'avère être bien pensé pour un montage facile.

### WiFi

La carte PCI-Express TP-Link TL-WDN4800 802.11a/b/g/n est gerée nativement : inutile de perdre du temps à configurer quoi que ce soit :)

### Bluetooth

Le Bluetooth est souvent le point faible des Hackintosh. Il est très compliqué de trouver des clés ou même des cartes PCI Bluetooth avec des chipsets gérés nativement sous OS X. Ils se comptent pratiquement sur les doigts de la main. Le modèle que propose **tonymacx86** censé être compatible nativement est malheureusement introuvable en Europe. Mais rien n'est perdu.. Après une petite recherche, il s'avère que la clé bluetooth [Inateck Nano dongle](http://www.amazon.fr/gp/product/B00F0CG0N4/ref=oh_details_o00_s00_i00?ie=UTF8&psc=1) disponible chez Amazon se base sur un chipset CSR8510, géré nativement (en tous cas sous Mavericks).

## Benchmark

Maintenant qu'on a tout monté, créé notre clé usb, installé Mavericks, qu'en est-il de cette configuration?

### Un peu d'overclocking

Notre Intel i5-4670k avec coefficient débloqué se révèle être un monstre de l'overclocking. Afin de pallier quelque peu notre manque de pêche face au Xeon E5 1650v2 utilisé dans le Mac Pro, on peut très bien pousser notre CPU de 3,8Ghz à 4,2Ghz sans même devoir toucher au voltage. Il reste stable, tant qu'il est bien refroidi (et le watercooling utilisé ici nous permet de le faire aisément).

Pareil pour la mémoire, la TridentX de chez G.Skill existe en plusieurs fréquences. Je n'ai réussi à avoir qu'un kit de 16Go en DDR3-1600, mais ses excellents timings d'origine et son système de refroidissement avec les ailettes nous permet encore une fois de pousser un peu la fréquence d'origine à 1866Mhz en leur donnant 1,60v au lieu des 1,50v d'origine, tout en ajustant les timings à -1 par rapport à ceux d'origine. Ca passe impec, et ça fait gagner quelques points au Benchmark :)

### **Mac Pro (Late 2013)** vs **MacPro6,1 (Hackintosh)**

**Info** | **Mac Pro (Late 2013)** | **MacPro6,1**
:----|:----|:----
Geekbench 3 Score | 3614 | 4307
Geekbench 3 Multicore Score | 14585 | 14264
Operating System	Mac | Mac OS X 10.9.2 | Mac OS X 10.9.2
Model	| Mac Pro (Late 2013) | MacPro6,1
Processor	| Intel Xeon E5-1620 v2 @ 3.70 GHz 1 processor, 4 cores, 8 threads | Intel Core i5-4670K @ 4.20 GHz 1 processor, 4 cores
Processor ID	| Genuine Intel Family 6 Model 62 Stepping 4 | GenuineIntel Family 6 Model 60 Stepping 3
L1 Instruction Cache | 32 KB x 4 | 32 KB x 2
L1 Data Cache	 | 32 KB x 4 | 32 KB x 2
L2 Cache | 256 KB x 4 | 256 KB x 2
L3 Cache | 10240 KB | 6144 KB
Motherboard |Apple Inc. Mac-F60DEB81FF30ACF6 MacPro6,1 | Apple Inc. Mac-F60DEB81FF30ACF6 x.x
BIOS | Apple Inc. MP61.88Z.0116.B05.1402141115 | Apple Inc. MP61.88Z.0116.B04.1312061508
Memory | 12288 MB 1867 MHz DDR3 | 16384 MB 1866 MHz DDR3

[Pour plus d'infos](http://browser.primatelabs.com/geekbench3/compare/509073?baseline=510297)

A 300pts près, on égale un Mac Pro au benchmark. Evidement, ça ne reste que des chiffres, et si vous êtes dans le milieu du graphisme, de l'édition vidéo ou tout ce qui de près ou de loin exploite les GPU, un véritable Mac Pro sera bien plus adapté à votre usage!

En tous cas, en tant que développeur, ce nouveau Mac Pro me convient parfaitement, et ses performances correspondent tout à fait à mon usage. Venant d'un MacBook Unibody de 2008 forcément, ça fait un choc :)