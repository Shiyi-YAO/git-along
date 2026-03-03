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
     我是制作squlette des fonctions的人, 对于我来说, 其实每一个filtres本身特别是我的这个部分(根据source来filtrer)并不难, 最难的是如何将不同的filtres条件结合起来进行filtrage, 最开始我对于我的fonction的entrée和sortie非常的困惑:
     ```
     filtre_xxx(item: dict, ...) → bool:
     filtrage(filtres, articles) -> list[dict]
     ```
     在feuille d'exercice 的帮助下, 我大概明白我们会需要三个fonctions de filtre, 然后一个fonction de filtrage qui combine tous les filtres en prennant une liste de filtres,但是这个时候我又非常困惑, comme écris dans la feuille `Si on utilise une liste de filtres, il faudra que toutes les fonctions de filtres utilisées prennent les mêmes entrées (nombre d’arguments et leurs types) et donnent la même sortie (type du résultat renvoyé). Vous pouvez vous inspirer des exemples donnés en cours pour voir comment vous pourriez procéder pour normaliser les arguments pour les différents filtres (source, catégories, etc).`
     
   - #### Solutions


- ### Rôle 3

## Merges (combinaison les 3 filtrages)

