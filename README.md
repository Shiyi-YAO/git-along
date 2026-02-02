# Séance 2

## Présentation du travail  

- ### Membre du groupe
```
r1 : ...
r2 : ...
r3 : ...
```

- ### Qu'est-ce qu'il fait notre projet
C'est un ... qui ...

  
- ### Façon d'utilisation
  
1. sans argument, san entrée standard
  ```
  python extraire_lexique.py
  ```

2. avec votre fichier en argument
  ```
  python extraire_lexique.py Corpus/*.txt
  ```

3. utiliser l'entrée standard avec cat
  ```
  cat Corpus/*.txt | python extraire_lexique.py
  ```

4. utiliser l'entrée standard avec ls
  ```
  ls Corpus/*.txt | python extraire_lexique.py
  ```

Vous pouvez également tester sur votre propre corpus juste en changeant l'argument "Corpus/*.txt" avec "Nom_de_votre_dossiers/*.txt"  




  
## Explication du tâche & Les difficulté et leur solustions

### Rôle 1
- #### tâche 1
Écrire une fonction qui lit les fichiers du dossier ./Corpus pour construire une liste de chaînes (list[str]), où chaque chaîne correspondra au contenu texte d’un fichier. Si un dossier contient 10 fichiers, la liste contiendra donc 10 éléments.

- #### tâche 2
permettre la lecture d’un corpus comme une liste de fichiers en arguments, en tapant par exemple :
```
  python extraire_lexique.py Corpus/*.txt
```

  
### Rôle 2
- #### tâche 1
Écrire une fonction prenant comme argument un corpus représenté par une liste de chaînes, et retournant un dictionnaire associant chaque mot à son nombre d’occurrences dans le corpus.
- #### tâche 2
Permettre la lecture du corpus depuis l’entrée standard, en donnant le contenu d’un document sur chaque ligne.
```
  cat Corpus/*.txt | python extraire_lexique.py
```

  
### Rôle 3
- #### tâche 1
Écrire une fonction prenant comme argument une liste de chaînes et retournant un dictionnaire associant chaque mot au nombre de documents dans lequel il apparaît.
- #### tâche 2
permettre de lister les chemins vers les fichiers du corpus sur l’entrée standard du programme en python :
```
  ls Corpus/*.txt | python extraire_lexique.py
```
