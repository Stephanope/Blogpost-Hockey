---
layout: post
title: Nettoyage des données
---

Pour travailler avec nos données plus facilement, nous avons créé un Dataframe panda. Ce dernier ressemble à ceci :

![Widget de navigation des événements](/images/donnees_nettoyees.png)

## Pistes de réflexion 

### Dans l'évantualité où l'attribut n'existait que pour les buts et que nous aimerions les avoir les tirs aussi, comment faire ?

Dans ce cas, nous devons réfléchir aux données disponibles dans notre fichier .json retourné par l'API et comment nous pouvons les traiter. Le champ de force, nous indique le nombre de joueurs et de gardiens présent au moment de l'action. Puisque les évènements sont en ordre chronologiques et que nous avons le timer à chaque action, nous pourrions stocker l'état des joueurs au cours de la partie dans des varaibles. Lorsqu'il y a une pénalité ou qu'un gardien est retiré, nous modifions l'état de la variable. Ainsi, à chaque tir, nous regardons nos variables pour indiquer si c'est un power-play ou non. Cette méthode est un peu lourde car nous devons regarder tous les évènements d'un match, mais si implémentée correctement, nous permet d'être sûr de l'état de la partie.

### Quelles caractéristiques supplémentaires pourrions-nous ajouter à notre Dataframe ?

- Rebonds, si deux tirs ont lieu un après l'autre très rapidement (ex. 3 secondes), nous pourrions considérer ce dernier comme un rebond. Cela est un paramètre assez important, car le gardien se retouve souvent hors de position dans ces là.

- Les tirs en contre-attaque, en observant l'évènement surgissant juste avant un tir, si ce dernier en défensive, nous pouvons considérer l'action comme une contre-attaque. Cela indique probablement que les défenseur n'ont pas eu le temps de se repositionner.

- L'angle du tir, certains angles sont plus ou moins difficiles à tirer. On peut donc déterminer la probabilité de tirer selon la position de l'attaquant.