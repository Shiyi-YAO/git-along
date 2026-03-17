# Séance 5

## Membres du groupe
```
r1 : ...
r2 : YAO Shiyi
r3 : ...
```

## Correction sur le travail du groupe précédent
Le code du groupe précédent peut être exécuté correctement. Cependant, les options de la commande de filtrage étaient assez longues. Par exemple, pour filtrer par date, il fallait saisir `--date_debut [xxxx-xx-xx]`. Ce n’est pas très pratique pour l’utilisateur. Nous avons donc raccourci toutes les options en utilisant des options courtes (un tiret suivi d’une lettre). Vous trouverez les détails de ces options dans le fichier `README.md`.

Selon la feuille d’exercice, les fonctions de lecture doivent retourner une liste de type `list[Article]` plutôt que `list[dict]`. Nous avons donc modifié les trois fonctions de lecture dans `rss_reader.py` :
- `def read_rss_xx(path: str) -> list[dict]` --> `def read_rss_xx(path: str) -> list[Article]`
- Les articles ajoutés dans la liste dans ces fonctions sont désormais directement des objets `Article`.
  ```
  articles = []
  for item in items:
     item_cats = categories(item)
     articles.append(
        Article(
           id=tag_text(item,'guid'),
           source=source,
           title=tag_text(item, 'title'),
           description=tag_text(item, 'description'),
           date=tag_text(item, 'pubDate'),
           categories=sorted(item_cats) if item_cats else sorted(global_categories1)
            ))
  ```

Dans `rss_reader.py`, la fonction `r3_feedparser` inclut une vérification permettant de déterminer si l’entrée était un fichier ou un dossier. Or, cette vérification est déjà effectuée dans `read_rss_dir`. Nous avons donc supprimé cette partie afin d’éviter toute redondance.

Le code ne séparait pas clairement les différentes fonctionnalités, ce qui le rendait difficile à lire. Nous avons donc ajouté des séparateurs tels que `#=============` ou `#-------------` afin de mieux structurer les différentes parties du programme.

## Séparation des codes et le squelette
Comme la feuille d’exercice précise clairement les fonctionnalités attendues dans chaque fichier, la séparation du code en trois fichiers `.py` ne nous a pas posé de difficulté particulière. Au début, nous hésitions toutefois à savoir s’il fallait modifier directement les trois fonctions `read_rss_xxx` dans `rss_reader.py` pour qu’elles renvoient `list[Article]`, ou bien les conserver et ajouter une fonction supplémentaire chargée de transformer une liste de dictionnaires en une liste d’objets `Article`. Cette seconde solution nous a finalement semblé inutilement compliquée. Nous avons donc choisi de modifier directement ces trois fonctions.

Dans la tâche de cette semaine, la partie la plus difficile a été de comprendre l’objectif global du code que nous devions finalement obtenir. Autrement dit, il fallait comprendre comment combiner toutes les fonctions existantes pour construire un programme permettant de traiter des fichiers via bash. Cet objectif n’était cependant pas très clairement indiqué dans la feuille d’exercice, et parfois certains consigne pouvait nous rend encore plus confus.

Par exemple :
- dans rss_parcours.py, soit lire des flux RSS et les parser, soit lire un corpus déjà sérialisé
  - Cependant, il n’était pas très clair comment obtenir ce corpus déjà sérialisé. Nous disposons bien d’une fonction `save_xxx(corpus: list[Article], output_file: Path)`, mais la question reste de savoir d’où provient cette `list[Article]`. Faut-il d’abord lire les fichiers RSS ? Et dans ce cas, faut-il également appliquer un filtrage lors de la lecture ?
- À titre de débuggage, ajoutez également dans datastructures.py une fonction main pour passer d’un format de sérialisation à un autre depuis bash (avec un parser d’arguments comme avant)
  - Pourquoi on fait ça ? Et cette conversion doit-elle faire partie de la version finale du programme ?

Finalement, nous avons décidé de faire le freestyle. Comme nous ne disposions pas encore d’un corpus déjà sérialisé, nous avons combiné les deux objectifs mentionnés précédemment : (1) obtenir un corpus sérialisé et (2) permettre la conversion entre différents formats de fichiers. Nous avons donc ajouté dans `rss_parcours.py` une fonctionnalité permettant de transformer des flux RSS en corpus sérialisé, avec l’option correspondante dans bash (`-o corpus.xml/json/pkl`). Cette fonctionnalité permet également de convertir un corpus sérialisé d’un format à un autre.

Nous avons intégré ces fonctionnalités à l’avance dans le squelette. Ainsi, il ne nous restait plus qu’à implémenter les deux fonctions de lecture et d’écriture, ce qui facilitera le merge final et garantira le bon fonctionnement du programme.

## Les difficultés et solustions

- ### Rôle 1


- ### Rôle 2
Pour ce rôle, nous devons utiliser le format JSON ainsi que le module `json` afin de sauvegarder et de recharger le corpus dans ou depuis un fichier, en proposant différents formats.

Les deux fonctions `save_json` et `load_json` sont simples à implémenter pour moi. En effet, les fichiers JSON stockent des données sous forme de dictionnaires (`dict`). Il suffit donc de convertir les objets `Article` en dictionnaires, et inversement. Pour cela, nous utilisons les méthodes vues en cours : `asdict(Article)` pour transformer un objet `Article` en dictionnaire, et `Article(**dict)` pour reconstruire un objet `Article` à partir d’un dictionnaire. Ensuite, nous utilisons les fonctions `dump` et `load` du module `json` pour écrire dans le fichier et lire son contenu.
```
liste_article.append(asdict(article)) # Article -> Dict
corpus.append(Article(**article))     # Dict -> Article

with open(output_file, "w") as f:
  json.dump(liste_article, f, indent=2) # Écrire list[Article] dans le fichier output_file

with open(input_file, "r") as f:
  liste_article = json.load(f)          # Lire list[dict] du fichier input_file
```

- ### Rôle 3

## Améliorations pendant la relecture

- ### Rôle 3
  (Lecteur `r2` : YAO Shiyi)

  1. J'ai appliqué la structure de squelette à ses parties pour faciliter la merge
  2. J'ai modifié sa façon d'écrire la fonctions (une seule fonction -> une fonction qui retourne une autre fonction) pour garantir le même entrée et sortie
  3. J'ai ajouté une autre option dans parseur et modifie ses fonction pour que l'utilisateur peut choisir le mode de match les catégories (soit tout soit un des catégories)
     ```
     parser.add_argument('-cm', '--categories-match', choices=['all', 'any'], default='all', help="Mode de correspondance pour --categories")
     ```

## Merges (combinaison les 3 filtrages)

Le merge final a été réalisée par r2. À cette étape, nous avons harmonisé la manière d’écrire et d’utiliser chaque parseur afin qu’ils suivent tous la même logique adoptée par r2. 
```
parser.add_argument('-d', '--date_debut', type=str, help="Format YYYY-MM-DD")
parser.add_argument('-f', '--date_fin', type=str, help="Format YYYY-MM-DD")
parser.add_argument("-s","--source", choices=("blast", "elucid", "bfm", "libération", "franceinfo", "lefigaro"), default=None)
parser.add_argument('-c', '--categories', type=str, nargs='+')
parser.add_argument('-cm', '--categories-match', choices=['all', 'any'], default='all', help="Mode de correspondance pour --categories")
```

Concernant la fonction de filtrage, nous avons choisi de conserver l’approche proposée par r1 en ajoutant une valeur `len(articles)` retounée pour indiquer le nombre des articles trouvés, cette combinaison est plus élégante que celle de r2.
```
def filtrage(filtres: list[bool], articles: list[dict]) -> tuple[list[dict], int]:

    for filtre in filtres:
        articles = list(filter(filtre, articles))
    return articles, len(articles)
```

Cependant, lors du merge, r2 a remarqué un problème : la fonction de r1 ne parvenait pas à traiter correctement le flux RSS de BFM lorsqu’on combinait les trois filtres.

En observant les données, nous avons identifié l’origine du problème :
```
blast, elucid ... : date: Sat, 08 Feb 2025 17:00:00 +0009 # Pour les autres sources, la ligne date se termine par un fuseau horaire numérique (par exemple +0009)
BFM               : date: Sun, 09 Feb 2025 11:03:33 GMT # Pour BFM, la date se termine par GMT
```
Or, dans le code initial, le format %z utilisé avec datetime.strptime ne reconnaît que les fuseaux horaires de type +xxxx, et ne fonctionne donc pas avec GMT.

Pour corriger cela, r2 a ajouté une légère modification à son code : il a intégré un second essai de parsing dans un bloc try/except, afin de gérer explicitement le format GMT
```
def filtre_date(debut: str, fin: str) -> Callable[[dict], bool]:
   ...
      try:
            date_article = datetime.strptime(
                date_str, "%a, %d %b %Y %H:%M:%S %z" # pour match Sat, 08 Feb 2025 17:00:00 +0009, %z ne match que +xxxx
      except ValueError:
            try:
                date_article = datetime.strptime(
                    date_str, "%a, %d %b %Y %H:%M:%S GMT" # pour match date: Sun, 09 Feb 2025 11:03:33 GMT
   ...
```
Grâce à cette adaptation, tous les articles, quelle que soit leur source, peuvent désormais être correctement filtrés selon le critère de date.
