# Séance 4

## Membres du groupe
```
r1 : CHALABI Sara Amina
r2 : YAO Shiyi
r3 : ...
```
Même répartition des rôles que la semaine 3
   
## Les difficultés et solustions

- ### Rôle 1
**Problème :** `ModuleNotFoundError: No module named 'feedparser'`
le module n'était pas installé. 
**Solution :** j'ai installer feedpaeser pour que mon script fonctionne.
```
sur bash

pip install feedparser
```
puis j'ai retesté avec la commande suivante et ça a bien fonctionné.
```
python3 rss_reader.py ~/Desktop/corpus -m regex
``` 
**Tag ajouté :** `SC-s3e2r1-fin`

- ### Rôle 2
   - #### Tâche
     Proposer un moyen de lire l’ensemble de l’arborescence du corpus, en suivant les mêmes principes que pour l’exercice précédent en utilisant le module `pathlib` et sa fonction `glob()`

     Voici le manuel vous pouvez trouver dans terminal(python) sur `glob()`
     ```
     Help on function glob in module pathlib:

     glob(self, pattern, *, case_sensitive=None)
       Iterate over this subtree and yield all existing files (of any
       kind, including directories) matching the given relative pattern.
     ```
     
   - #### Difficultés
     1. Comme `glob()` est une fonction assez puissante, cette partie du projet ne m’a pas posé trop de difficultés. En revanche, au début, le fait de ne consulter que l’aide de `help(Path.glob)` dans l’interface interactive de Python du terminal ne m’a pas permis de bien comprendre comment utiliser concrètement cette fonction. Par exemple, je ne savais pas exactement quelle était la forme de l’appel de la fonction : est-ce que c’était comme `glob(chemin_dossier, pattern)`, et si le paramètre pattern fonctionnait de la même manière qu’un pattern en expressions régulières (regex)? Une autre difficulté importante était de comprendre sous quelle forme la fonction renvoie le résultat : s’agit-il d’une liste ou d’un autre type d’objet ?
    
   - #### Solutions
     1. J’ai demandé à `ChatGPT` des explications sur l’utilisation concrète de `glob()` :
        ```
        from pathlib import Path

        p = Path("chemin") # Path nous permet de manipuler les chemins de fichiers de manière orientée objet, au lieu de recourir des str
        files = p.glob("*.xml") # Récupère tous les fichiers .xml du dossier p
        ```
        Ensuite, j’ai testé ce code avec mon propre dossier chemin_dossier afin d’observer le type de résultat renvoyé :
        ```
        from pathlib import Path
        p = Path("Corpus")
        files = p.glob("*.xml")
        print(files)
        >>> <generator object Path.glob at 0x100b61580> # Il s’agit d’un objet itérable, et non d’un ensemble de résultats qui peut être affiché
        ```
        Comme il s'agit un itérateur, on peut utiliser une boucle `for` pour prendre les chemins_fichiers :
        ```
        for file in files:
          chemin_file = file
        ```


- ### Rôle 3


## Choix de merge
Pour le merge de cet exercice, nous avons choisi de reprendre l’approche de r2 pour combiner les trois fonctions de lecture ainsi que la fonction `main`, dans son approche, nous pouvons trouver:
   - une configuration de parseur et la fonction de combinaison similaire à l'exo1
     ```
     def lire_arborescence(chemin_dossier, lire_dossier="iterdir"):
   
       if lire_dossier == "iterdir":
           return lire_arborescence_pathlib_iterdir(chemin_dossier)
       elif lire_dossier == "glob":
           return lire_arborescence_pathlib_glob(chemin_dossier)
       elif lire_dossier == "os":
           return lire_arborescence_os(chemin_dossier)
       else:
           raise ValueError(f"façon de lire le dossier inconnue : {lire_dossier}")
     ...
     parser.add_argument(
        "-ld", "--lire_dossier",
        choices=["iterdir", "glob", "os"],
        default="iterdir",
        help="Façon de lire le dossier (défaut : iterdir)"
     )
     ```
   - une fonction supplémentaire qui permet de sauvegarder les résultats dans un fichier lorsque le traitement porte sur un dossier contenant un grand nombre de fichiers, afin d’éviter un affichage trop volumineux dans le terminal.
     ```
     def sauvegarder_articles_txt(articles, chemin_fichier, dossier_sortie):
      
        dossier_sortie = Path(dossier_sortie)
        dossier_sortie.mkdir(exist_ok=True) # la façon de créer un dossier dans python
      
        nom_fichier_sortie = Path(chemin_fichier).stem # prendre le nom du fichier sans l'extension'
        fichier_sortie = dossier_sortie/f"{nom_fichier_sortie}.txt"
      
        with open(fichier_sortie, "w") as f:
           for article in articles:
              for k, v in article.items():
                 f.write(f"{k}: {v}\n")
              f.write("\n")
     ```
   - une condition `if` qui permet de déterminer automatiquement si l’entrée fournie correspond à un fichier ou à un dossier
     ```
     if chemin.is_file(): # si entrée est un seul fichier, afficher directement dans terminal
        articles = lire_rss(args.fichier, args.methode) # s'il s'agit un seul fichier, on va juste choisir la méthode
        for article in articles:
           for k, v in article.items():
              print(f"{k} : {v}")
           print()
   
      elif chemin.is_dir(): # si entrée est un dossier, sauvgarder les résultats dans un dossier des fichiers texte
        chemin_fichiers_iter = lire_arborescence(args.fichier, args.lire_dossier) # on choisir d'abord la façon de lire le dossier pour obtenir une liste ou un itérateur qui permet de parcourir les chemins des fichiers
        dossier_sortie = args.methode
   
        for chemin_fichier in chemin_fichiers_iter:
           articles = lire_rss(chemin_fichier, args.methode)
           sauvegarder_articles_txt(articles, chemin_fichier, dossier_sortie)
     ```
