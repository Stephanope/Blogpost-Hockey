---
layout: post
title: IFT6758 Visualisation Simple
---
# Types de tirs : tirs vs buts
![Nombre de tirs(barres) + nombre de buts(courbe)](../images/buts_tirs_2023.png)
Le graphique montre que les wrists shots sont de loin les plus fréquent. Logiquement c'est donc aussi celui qui compte le plus de buts. Cependant les types de tirs que nous considérerons comme les plus dangereux sont ceux qui ont les meilleures conversions. Si on observe que la courbe de buts est pas mal au dessus de la barre on considère que c'est un tirs dangeureux. Par exemple pour cette saison on considère les snaps ou les tip-ins comme les tirs les plus dangereux.
### Pourquoi ce graphique?
Superposer le volume de tirs et le nombre de buts permet de distinguer ce qui est courant de ce qui est efficace sans confondre "beaucoup de buts" avec "beaucoup de tentatives".
# Distance vs probabilité de but
![2018-19](../images/proba_but_2018.png)
![2019-20](../images/proba_but_2019.png)
![2020-21](../images/proba_but_2020.png)
Pour les trois saisons la tendance reste la même, plus un tir est loin moins il a de chances d'être un but. 
### Changements entre saisons?
Pas vraiment, on remarque juste qu'il y a plus de variabilité lorsque l'effectif des bins est plus petit mais la forme de la courbe et des bins reste plutôt stable.
### Pourquoi ce graphique?
La courbe montre directement la probabilité et l'histogramme du nombre de tirs indique où l'estimation est la plus fiable.
### Calcul de la distance
Puisque pour la grande majorité des tirs xCoord et yCoord sont disponibles, nous avons choisi de calculer la distance simplement via une distance euclidienne. On trouve donc l'écart des coordonnées du tirs (on prend la valeur absolue pour x afin de standardiser le côté d'attaque) et du but et on considère cela comme distance. 
```python
def compute_shot_distance(x_coord, y_coord, goal_x=89.0, goal_y=0.0):
    """Distance euclidienne entre le tir et le filet. On prend abs(x) pour standardiser la patinoire."""
    distance = np.sqrt((goal_x - abs(float(x_coord)))**2 + (float(y_coord) - goal_y)**2)
    return distance
```

# Probabilité de but selon la distance + le type de tir
![Nombre de tirs(barres) + nombre de buts(courbe)](../images/buts_dist_type_2023.png)
Ce graphique démontre que la distance est beaucoup plus influente pour la probabilité de but. Les taux de buts sont clairement plus élevés près du filet et chutent rapidement. Le graphe permet aussi de voir que certains type de tirs marchent mieux de proche que de loin comme les tip ins et d'autres inversement, certains semble plus efficace de loin comme les deflections et finalement d'autres marchent assez bien de proche et loin comme les snaps et slaps shots. On remarque aussi le sommet du graphique, les slaps shots de 10 à 20 pieds du buts semblent être le type de tir le plus dangereux parmis tous.
### Remarque :
Pour tous les graphiques de ce blog si l'on souhaite grapher une autre saison que celle affichée on peut le faire avec le notebook simple_viz.ipynb.

