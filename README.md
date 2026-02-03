# Séance 2

### Membre du groupe
```
r1 : ...
r2 : ...
r3 : YAO Shiyi
```
   
### Explication du tâche & Les difficulté et solustions

- #### Rôle 1


- #### Rôle 2


- #### Rôle 3
   - Tâche 1 : compter le nomdre de documents par mot (fonction `compter_nb_doc(corpus)`)

      - Difficulté

        la principale difficulté de cette partie est d’éviter de compter plusieurs fois un même mot dans un document et de conserver la même liste de mots que pour le rôle 2. Sinon, la longueur des listes est différente et il devient impossible d’afficher correctement les résultats.
        
      - Solution

        pour garantir la cohérence avec le rôle 2, la même méthode d’extraction des mots a été utilisée (`re.findall`) au lieu de ma propre façon (`split`). Les mots sont ensuite stockés dans un ensemble (`set`) afin d’éviter les doublons dans un même document.
        ```
        r2 : mots = re.findall(r'\w+', texte.lower())  ---> r3 : mots_doc = set(re.findall(r'\w+', doc.lower()))
        ```
        mais lors des tests je noté qu'il y a des mots "chiffrés" (comme 000, 18273, wyeu373684), donc j'ai ajouté une ligne de code pour ignorer les no_alphabet
        ```
        mots_doc = set(re.findall(r'\w+', doc.lower())) 
        mots_doc = [m for m in mots_doc if m.isalpha()] # .isalpha() --> seulement les alphabet peut etre mis dans la liste des mots
        ```
  
   - Tâche 2 : Effectuer le script par l'entrée standard - ls (`ls Corpus/*.txt | python extraire_lexique.py`)

      - Difficulté
    
        Ce que je trouve le plus difficile ici, c’est qu’avec une seule ligne de commande, je n’arrive pas à comprendre ou imaginer clairement ce que je dois faire concrètement (autrement dit, le passage de la description textuelle de la tâche à sa mise en œuvre pratique)
        
      - Solution
    
        Au lieu d’analyser cette commande dans son ensemble, je me concentre d’abord sur la partie gauche, à savoir(`ls Corpus/*.txt`). En effet, la sortie de cette commande sera utilisée comme entrée de mon script.

        Pour comprendre précisément ce qui est transmis via l’entrée standard, j’exécute (`ls Corpus/*.txt > files.txt`) dans le terminal et j'obtiens : 
           ```
           Corpus/01.txt
           Corpus/02.txt
           Corpus/03.txt
           ...
           ```
        Il s’agit donc clairement d’une liste de noms de fichiers, un par ligne, la fonction (`lire_corpus(fichiers)`) attend en paramètre une liste de chemins de fichiers donc la seule chose à faire est de transformer ces lignes lues depuis l’entrée standard en une liste Python.
        
        Pour le faire, il suffit de créer une liste vide, puis de parcourir chaque ligne de stdin à l’aide d’une boucle, et d’ajouter chaque chemin à la liste :
         ```
         liste_chemins = []
         for line in sys.stdin:
            liste_chemins.append(line)
         ```
        Cependant, chaque ligne se termine par (\n). Il est donc nécessaire de le supprimer avant l’ajout à la liste, en utilisant la méthode (`.strip()`) :
         ```
         liste_chemins.append(line.strip())
         ```
        Une fois cette liste de chemins construite, il est alors possible de réutiliser la fonction (`lire_corpus(fichiers)`), ce qui permet d’obtenir un corpus, identique à celui obtenu dans l’exercice 2, et pouvant être passé en paramètre aux fonctions compter_occurrences(liste_textes) et compter_nb_doc(corpus)
        ```
        corpus = lire_corpus(liste_chemins)
        return corpus
        ```













