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
        Cependant, chaque ligne se termine par (\n). Il est donc nécessaire de le supprimer avant l’ajout à la liste, en utilisant la méthode `.strip()` :
         ```
         liste_chemins.append(line.strip())
         ```
        Une fois cette liste de chemins construite, il est alors possible de réutiliser la fonction `lire_corpus(fichiers)`, ce qui permet d’obtenir un corpus, identique à celui obtenu dans l’exercice 2, et pouvant être passé en paramètre aux fonctions compter_occurrences(liste_textes) et compter_nb_doc(corpus)
        ```
        corpus = lire_corpus(liste_chemins)
        return corpus
        ```


- #### Merges vers main

   - Tâche 1 (`git merge s2ex2SY` : r3)

     ```
     # --- affichage des résultats --- #
     def afficher_res(corpus):
        nb_occ = compter_occurrences(corpus)
        nb_doc = compter_nb_doc(corpus)
      
        with open("./resultats.tsv", "w", encoding="utf-8") as f:
           f.write("mot\toccurrence\tnb_document\n")
           for mot in sorted(nb_occ.keys()):
              f.write(f"{mot}\t{nb_occ[mot]}\t{nb_doc.get(mot, 0)}\n")

     # --- l'entrée pour exo2 --- #
        def entrée_exo2(): 
           fichiers = []
           for i in range(1,31,1):
              if i < 10:
                 fichiers.append(f"Corpus/0{i}.txt")
              else:
                 fichiers.append(f"Corpus/{i}.txt")
           return fichiers
      
     fichiers = entrée_exo2()
     corpus = lire_corpus(fichiers)
     afficher_res(corpus)
     ```
     Comme l’exécution de cet exercice se fait à l’aide de la commande `python extraire_lexique.py`, et que le code de l’exercice 3 est écrit dans un seul et même script, il n’est pas nécessaire de se soucier de l’importation des fonctions. Nous pouvons donc utiliser une écriture Python simple et directe pour définir nos fonctions.
     
     这里的affichage我们选择用一个tsv表格(`resultats.tsv`)来展示, 如果只是在终端显示的话, 会出现每一列无法对齐的现象, 会有点难看

     (`def entrée_exo2()`)是为了得到fonction(`lire_corpus(fichiers)`), 这里我们选择用一个boucle来得到一个liste des noms du fichiers而不是简单地将它们直接写到一个liste里面(`['Corpus/01.txt', 'Corpus/02.txt', ...]`)是为了防止之后如果我们增加corpus的数量, 比如到一百, 这时就只用改range范围和if条件即可, 而不用一个一个地填
     
   - Tâche 2 (git merge s2ex3SY` : r3)
 
     coming soon












