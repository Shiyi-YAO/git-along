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
     Comme l’exécution de cet exercice se fait à l’aide de la commande (`python extraire_lexique.py`), et que on décide d'écrire le code de l’exercice 3 dans le même script, il n’est pas nécessaire considérer le cas où il faut (`import`). Nous pouvons donc utiliser une écriture Python simple et directe pour définir nos fonctions.
     
     Pour l’affichage des résultats, nous avons choisi d’utiliser un tableau au format TSV (resultats.tsv). Un affichage direct dans le terminal rendrait les colonnes difficiles à aligner.

     La fonction (`def entree_exo2()`) a pour objectif de fournir le paramètre à la fonction (`lire_corpus(fichiers)`). Pour cela, nous avons choisi d’utiliser une boucle afin de construire automatiquement une liste de noms de fichiers, plutôt que de les écrire manuellement dans une liste (`['Corpus/01.txt', 'Corpus/02.txt', ...]`), cette approche permet de rendre le programme plus flexible : si le nombre de fichiers du corpus augmente par la suite (par exemple jusqu’à une centaine de fichiers), il suffira de modifier l’intervalle de la fonction range ou d’ajuster une condition, sans avoir à ajouter chaque chemin de fichier individuellement.


   - Tâche 2 (`git merge s2ex3SY` : r3)
 
     Cette partie a été la plus difficile au départ, car nous ne maîtrisions pas encore très bien l’utilisation du module (`argparse`) et dans l’exercice 3, nous avons implémenté trois façons différentes de lire l’entrée standard (`stdin`), à l’aide de trois fonctions distinctes : (`lire_arg(fichiers)`), (`lire_cat()`) et (`lire_ls()`).

     Notre idée initiale était donc de déterminer, à partir de l’entrée fournie par l’utilisateur, quelle fonction devait être utilisée pour lire les données depuis (`stdin`) :
      - `lire_arg(fichiers)` correspond au cas où l’utilisateur exécute la commande (`python extraire_lexique.py Corpus/*.txt`) ; dans ce cas, les chemins des fichiers sont passés comme arguments au parser.
      - `lire_cat()` correspond au cas où l’entrée provient de (`cat Corpus/*.txt`), dont la sortie contient le contenu de tous les fichiers.
      - `lire_ls()` correspond au cas où l’entrée provient de (`ls Corpus/*.txt`), dont la sortie est une liste de noms de fichiers.
     ```
     def choisir_stdin():

        my_parser = argparse.ArgumentParser(description="Extraire la fréquence des mots et la fréquence des documents d'un corpus textuel.")
        my_parser.add_argument("fichiers", nargs="*", help="Fichiers du corpus (si absent, lecture depuis l'entrée standard)")

        args = my_parser.parse_args()

     # Plusiseurs cas à vérifier selon l'entrée

     # 1. s'il y a un argument
     if args.fichiers: # s'il y a un argument
        return lire_arg(args.fichiers)

     lignes = [ligne.strip() for ligne in sys.stdin if ligne.strip()]

     # 2. aucun argument, aucune entrée standard
        if not lignes:
           fichiers = entree_exo2()
           return lire_corpus(fichiers)

     # 3. s'il s'agit un entrée standard
     # 3.1 ls : une liste de fichiers
        if all(os.path.isfile(ligne) for ligne in lignes):
           return lire_ls()

     # 3.2 cat : contenu des docs
        return lire_cat()
     ```
     我们在其中加入了(aucun argument, aucune entrée standard)这个条件来让用户可以使用exo2的命令(python extraire_lexique.py)来运行代码
     这个方法看起来是很完美, 但当我们运行的时候, 我们的tsv中只有表头而没有词语这些, 然后为了回答这个问题, 我们询问了ChatGPT, 这是因为我们在这个fonction中已经读取了stdin的内容, 导致我们exo3的三个fonctions没有内容可读, 所以我们就在想是否有一个办法可以直接判断命令行本身(比如是cat还是ls还是以参数形式出现), 一下是我们的最终版本:
     ```
     def choisir_entree():

        # Plusiseurs cas à vérifier selon l'entrée
        my_parser = argparse.ArgumentParser(description="Extraire la fréquence des mots et la fréquence des documents d'un corpus textuel.")
        my_parser.add_argument( "--mode", choices=["arg", "cat", "ls"], help="Mode de lecture du corpus")
        my_parser.add_argument("fichiers", nargs="*", help="Fichiers du corpus (utilisé avec --mode arg)")

        args = my_parser.parse_args()

        # 1. aucun argument, aucune entrée standard
        if args.mode is None:
           fichiers = entrée_exo2()
           return lire_corpus(fichiers)
   
        # 2. s'il y a un argument
        if args.mode == "arg":
           return lire_arg(args.fichiers)
   
        # 3. cat : contenu des docs
        elif args.mode == "cat":
           return lire_cat()
   
        # 4. ls : une liste de fichiers
        elif args.mode == "ls":
           return lire_ls()

     if __name__ == "__main__":
        corpus = choisir_entree()
        afficher_res(corpus)
     ```







