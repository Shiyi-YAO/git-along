# Séance 5

## Membres du groupe
```
r1 : ...
r2 : YAO Shiyi
r3 : ...
```

## Correction sur le traval du groupe précédent
Le code du groupe précédent peut être exécuté correctement. Cependant, les options de la commande de filtrage étaient assez longues. Par exemple, pour filtrer par date, il fallait saisir `--date_debut [xxxx-xx-xx]`. Ce n’est pas très pratique pour l’utilisateur. Nous avons donc raccourci toutes les options en utilisant des options courtes (un tiret suivi d’une lettre). Vous trouverez les détails de ces options dans le fichier `README.md`.

根据这次的feuille d'exercice, 由于我们需要在lire un fichier之后获得list[Article] au lieu de list[dict], 所以我们对`rss_reader.py`中三个lecture的focntion做出了一下修改:
- `def read_rss_xx(path: str) -> list[dict]` --> `def read_rss_xx(path: str) -> list[Article]`
- 在focntion内部将article存入list的时候, 直接存入生成的Article对象
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

## Les difficultés et solustions

- ### Rôle 1


- ### Rôle 2
这两个fonction de lecture和ecriture `save_json`, `load_json`对我来说还挺简单的, 因为json文件需要存入Dict, 所以

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
