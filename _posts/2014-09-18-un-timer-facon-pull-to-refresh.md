---
layout: post-no-feature
title: "Un timer façon pull-to-refresh"
description: "Comment réinventer les usages des utilisateurs sous iOS."
category: articles
comments: true
tags: [mac, osx, unix, terminal, vim, zsh, oh-my-zsh]
---

![timer-ptr]({{ site.url }}/images/timer-ptr.gif)

> Réutiliser les habitudes des utilisateurs est la clé.

Durant le développement d'une application ayant pour domaine les résultats en
direct d'évènements sportifs, il me fallait faire un choix crucial : laisser les
utilisateurs mettre à jour le direct par eux-mêmes ou instaurer un système de
mise à jour automatique.

Evidemment la deuxième solution est la plus intéressante dans mon cas, mais il
me fallait résoudre un problème du côté de l'interface utilisateur : comment
indiquer que l'application se mettait automatiquement à jour toutes les minutes,
et comment transcrire cela d'une manière visuelle qui ne bousculerait ni le
design de mon application, ni les habitudes des utilisateurs d'iOS.

## Le Pull-to-refresh

Au travers des années, et grâce en premier lieu à l'application
[Tweetie](http://en.wikipedia.org/wiki/Tweetie) qui a instauré ce geste
désormais ancré chez les possesseurs de smartphone, le "pull-to-refresh"
littéralement "tirez pour mettre à jour" est devenue une habitude qu'on essaye
un peu partout. Si ce geste s'est aussi bien généralisé, c'est aussi parce qu'il
a été adopté par de nombreux développeurs dans leurs applications. Les
utilisateurs ont donc désormais l'habitude de tirer sur les table views dans
l'espoir de mettre à jour ou rafraichir les informations.

L'idée c'était de leur dire : le direct se met à jour automatiquement, ne vous
en préoccupez-pas. Pour cela, il fallait utiliser ce geste et réinventer son
indication. J'ai donc intégré un timer avec un décompte de 60 secondes en lieu
et place d'une animation qui permet de recharger les données.

L'autre défi, c'était de faire ça de la manière la moins intrusive possible dans
le code. Je n'avais pas envie de sous-classer UITableView ni de modifier une
grosse partie de mon code, surtout que l'action en elle même n'entraîne aucune
autre action, l'animation ne doit être que passive.

## Intégration

En se basant sur la librairie
[DDHTimerControl](https://github.com/dasdom/DDHTimerControl), la moitié du
travail pour obtenir un timer avec un décompte animé est déjà fait. Seulement,
c'est pas très sexy d'origine, il va donc falloir bien le customiser selon ce
que vous souhaitez obtenir au final.

On initialise le composant dans *viewDidLoad* :

{% highlight objc linenos %}
_timerControl = [DDHTimerControl timerControlWithType:DDHTimerTypeSolid];
_timerControl.translatesAutoresizingMaskIntoConstraints = NO;
_timerControl.color = [UIColor colorWithHexString:@"444444" alpha:0.2];
_timerControl.ringWidth = 3;
_timerControl.minutesOrSeconds = 60;
_timerControl.userInteractionEnabled = NO;
UIView *topView = (UIView *)[self.tableView viewWithTag:10];
_timerControl.frame = CGRectMake(0, 0, 66, 66);
_timerControl.center = CGPointMake(topView.frame.size.width/2, (topView.frame.size.height-20)/2);
[_timerControl addTarget:self action:@selector(valueChanged:) forControlEvents:UIControlEventValueChanged];
[topView addSubview:_timerControl];
            
NSTimer *timer = [NSTimer scheduledTimerWithTimeInterval:0.1 target:self selector:@selector(changeTimer:) userInfo:nil repeats:YES];
[[NSRunLoop currentRunLoop] addTimer:timer forMode:NSRunLoopCommonModes];
        
_endDate = [NSDate dateWithTimeIntervalSinceNow:12.0f*60.0f];
{% endhighlight %}

La petite astuce ici est d'utiliser NSRunLoopCommonModes comme mode à appliquer
pour faire tourner le timer afin que la vue puisse être mise à jour dans un
thread différent histoire de pouvoir toujours avoir le décompte des secondes
même si l'utilisateur interagit avec l'interface (ici typiquement, tirer la
TableView). 

On a instancié un NSTimer qui appellera notre méthode *changeTimer* qui va
mettre à jour le décompte, appelée toutes les 0,1 secondes :

{% highlight objc linenos %}
- (void)changeTimer:(NSTimer*)timer {
    NSTimeInterval timeInterval = [self.endDate timeIntervalSinceNow];
    NSLog(@"timeInterval: %f, minutes: %f", timeInterval, timeInterval/60.0f);
    _timerControl.minutesOrSeconds = ((NSInteger)timeInterval)%60;
}
{% endhighlight %}

Le selector *valueChanged* ajouté sur le timerControl est appelé dès que la
valeur du timer change, ce qui nous permettra de lancer l'action à effectuer
lorsque la minute sera passée :

{% highlight objc linenos %}
- (void)valueChanged:(DDHTimerControl*)sender {
    NSLog(@"value: %d", sender.minutesOrSeconds);
    if (sender.minutesOrSeconds == 0)
        [self reloadData];
}
{% endhighlight %}

Toute l'idée est donc d'ajouter notre vue *timerControl* en tant que subView
dans la topView qu'on a placé au dessus de la TableView (ici la vue sélectionnée
en vert).

![xcode-screen1]({{ site.url }}/images/xcode-screen1.png)

Afin de cacher la vue et la placer au-delà de notre TableView, on va utiliser
les propriétés insets des TableView pour décaler leur position initiale.

{% highlight objc linenos %}
[self.tableView setContentInset:UIEdgeInsetsMake(-65, 0, 0, 0)]; // Hide 65px - height of my topView (need to pull to reveal the timer)
{% endhighlight %}

### Customisation

A l'ouverture du fichier header mauvaise nouvelle : le développeur ne propose
quasiment pas de méthodes (avec très peu de variables exposées) pour modifier
l'apparence du timer. Résultat... il va falloir modifier directement dans la
librairie, ce qui est vraiment à éviter. En théorie, une librairie bien écrite
ne devrait jamais avoir besoin d'être modifiée pour en personnaliser les
propriétés.

Pour se débarrasser de l'extrémité du cercle qui indique le décompte, il suffit
d'aller ouvrir le fichier *DDHTimerControl.m* et de passer en commentaire la
ligne suivante :

{% highlight objc linenos %}
[handlePath fill];
{% endhighlight %}

Pour redéfinir le label qui indique les secondes, j'ai modifié les lignes
suivantes afin de centrer le décompte :

{% highlight objc linenos %}
_minutesOrSecondsLabel.frame = CGRectMake(0, 0, 30, 30);
_minutesOrSecondsLabel.center = CGPointMake(frame.size.width/2, frame.size.height/2);
{% endhighlight %}

On modifie la font et la taille de celle-ci pour le décompte des secondes :

{% highlight objc linenos %}
NSDictionary *labelFontAttributes = @{NSFontAttributeName:[UIFont fontWithName:@"Lato-Bold" size:23],
                           NSForegroundColorAttributeName:self.color,
                            NSParagraphStyleAttributeName:labelStyle};
{% endhighlight %}
