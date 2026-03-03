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







