# Séance 5

## Membres du groupe
```
r1 : ...
r2 : YAO Shiyi
r3 : ...
```

## Commentaires (pour le traval du groupe précédent)
Le groupe précédent n’a pas fait l'unification, par conséquent, le format des résultats renvoyés par chaque rôle est différent. 
De plus, aucun nettoyage ni normalisation des fichiers n’a été effectué, ce qui fait que le résultat final ne correspond pas à nos attentes 
(les descriptions ne contiennent pas de sous-balises et les catégories n’ont pas été entièrement récupérées).

J’ai également constaté qu’il n’y avait pas de squelette commun avant d'écrire les fonctions par chaque rôle, 
ce qui a conduit chacun à implémenter les fonctions à sa manière. 
Par exemple, r1, r2 définissaient directement dans leurs fonctions les paramètres nécessaires à l’article (id, source, etc.), 
tandis que r2 avait créé une fonction dédiée à la définition des données avant de les réutiliser dans ses propres fonctions. 
Cette absence d’harmonisation rend l’ensemble du code confus et introduit plusieurs blocs inutiles.

## Les difficultés et solustions

- ### Rôle 1


- ### Rôle 2
   - #### Tâche
     écrire une fonction de filtrage en fonction de la ou des sources (noms des journaux/sites comme BFM, Libération, Blast. . .) ainsi qu’assurer l’unicité des articles dans le résultat final.
     
   - #### Difficultés
     Comme j’étais chargée de faire le squelette des fonctions, en réalité chaque filtre pris séparément — surtout le mien (le filtrage par source) — n’était pas très compliqué.
 
     Ce qui était vraiment difficile, c’était de trouver la façon d'écrire les fonctions de filtres et de comprendre comment combiner plusieurs filtres ensemble pour faire un filtrage cohérent.
 
     Au début, j’étais assez perdue par rapport aux entrées et aux sorties de mes fonctions :
     ```
     filtre_date(item: dict, date_début, date_fin) → bool:
     filtre_source(item: dict, source) → bool:
     filtre_categories(item: dict, categories) → bool:
     
     filtrage(filtres, articles) -> list[dict]
     ```
     Avec l’aide de la feuille d’exercice, j’ai fini par comprendre qu’on aurait besoin de trois fonctions de filtre, puis d’une fonction de filtrage qui combine tout ça en prenant une liste de filtres en paramètre.

     Mais là, je me suis retrouvée encore plus confuse, surtout à cause de cette phrase dans la feuille :
     ```
     Si on utilise une liste de filtres, il faudra que toutes les fonctions de filtres utilisées prennent les mêmes entrées (nombre d’arguments et leurs types) et donnent la même sortie (type du résultat renvoyé). Vous pouvez vous inspirer des exemples donnés en cours pour voir comment vous pourriez procéder pour normaliser les arguments pour les différents filtres (source, catégories, etc).
     ```
     Et là je me suis posé plein de questions :
     - Concrètement, c’est quoi un élément dans cette liste de filtres ?
     - Est-ce que ce sont les arguments donnés par l’utilisateur ?
     - Est-ce que ce sont les valeurs booléennes retournées par les fonctions ?
     - Et surtout, comment faire pour que toutes les fonctions aient les mêmes entrées, alors qu’elles n’ont pas du tout besoin des mêmes paramètres ?

     Franchement, c’est cette idée d’uniformiser les arguments qui m’a le plus posé problème.
     
   - #### Solutions
     Je me suis inspirée de l’exemple donné en cours :
     ```
     from typing import Callable
     def adder(how_much: int) -> Callable[[int], int]:
        def add(to: int) -> int:
           return to + how_much
        return add
     ```
     En observant ce bloc de code, j’ai remarqué qu’il s’agissait d’une fonction qui retourne une autre fonction où je peux séparer mes arguments.
 
     À partir de là, j’ai compris que je pouvais appliquer le même principe à mes fonctions de filtre. Voici l'idée:
     - Les paramètres spécifiques à chaque filtre (`date`, `source`, `catégories`) sont définis dans la fonction externe.
     - Le paramètre commun à tous les filtres (`item: dict`) est placé dans la fonction interne.
     Ainsi, la fonction retournée prend toujours le même type d’argument `(item: dict)` et renvoie toujours un booléen. Cela permet donc d’uniformiser les entrées, comme demandé dans l’énoncé.

     Voici la structure que j’ai adoptée :
     ```
     from typing import Callable

     def filtre_source(source) -> Callable[[dict], bool]:
        def filtre(item: dict) -> bool:
           pass

     def filtrage(filtres, articles) -> list[dict]:
        pass

     def main():
        ...
        filtres = []
        if args.source:
           filtres.append(filtre_source(args.source))
     ```
     Dans main, il suffit donc d’ajouter chaque filtre dans la liste filtres. Ensuite, la fonction filtrage applique successivement chaque filtre à chaque article.

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
Or, dans le code initial, le format %z utilisé avec datetime.strptime ne reconnaît que les fuseaux horaires de type +xxxx ou -xxxx, et ne fonctionne donc pas avec GMT.

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







