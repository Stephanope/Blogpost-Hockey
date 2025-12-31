---
layout: post
title: Acqusition des données
---

Pour acquérir les données des parties de hockey, l'API de la NHL nous permet de télécherger tout ce dont nous avons besoin. La requête suivante permet d'acquérir toutes les données d'une partie en particulier : 

##### https://statsapi.web.nhl.com/api/v1/game/[GAME_ID]/feed/live/

Nous avons écris la méthode suivante pour récupérer et télécharger les données d'une partie en particulier. Pour éviter d'utiliser l'API inutilement, nous vérifions si nous avons déjà les données téléchargées. Si c'est le cas, nous les récupérons localement dans le fichier data.

```python
def get_game(game_year, game_type, game_number):
    """Récupère les infos d'une partie à partir de la chache ou de 
    l'API

    Paramètres:
    game_year (int): Année ou la partie a eu lieu
    game_type(int): Type de la partie (1 = pré-saison, 
    2 = saison régulière, 3 = playoffs, 4 = all-star)
    game_numer (in t): Numéro de la partie

    Retourne:
    data (dict): Données de la partie spécifiée
    """

    # Conversion des int en string pour l'API
    str_year = str(game_year)
    str_type = str(game_type).zfill(2) 
    str_num = str(game_number).zfill(4) 

    # Création de la requête GET
    game_ID = str_year + str_type + str_num 
    url = f"https://api-web.nhle.com/v1/gamecenter/{game_ID}
    /play-by-play"  

    # Création du path du fichier qui va stocker les données
    file_name = game_ID + ".json" 
    base_path = DATA_DIR / str_year 
    base_path.mkdir(parents=True, exist_ok=True) 
    complete_path = base_path / file_name 

    # Si les données sont présentes localement on les récupère 
    #directement
    if complete_path.is_file():
        with open(complete_path, 'r', encoding='utf-8') as f:
            return json.load(f)
    # Sinon on fait une requête à l'API de la NHL
    else:   
        response = requests.get(url) 
        if response.status_code == 200: 
            data = response.json()     
            with open(complete_path, 'w', encoding='utf-8') as f: 
               json.dump(data, f, indent=4) 
            return data
        else: 
            print(f"Erreur {response.status_code} lors du 
            téléchargement.") 
            return None 
```

Pour récupérer toutes les données de toutes les parties d'une saison régulière, nous avons créé la fonction suivante. Puisque le nombre de parties d'une saison régulière ne peut pas dépasser 1351, nous sommes sûrs de toutes les récupérer. Nous avons choisi l'ignorer les données des autres types de partie pour le Milestone 1.

```python
def fetch_full_year_regular(year):
    """Récupère les données des parties d'une saison régulière 
    complète

    Paramètre:
    year (int): Année de la saison régulière
    """
    for i in range(1, 1351): 
        data = get_game(year, 2, i) 
        if data is None:
            print(f"Arrêt à la partie {i}, fin de la saison 
            détectée.")
            break
```