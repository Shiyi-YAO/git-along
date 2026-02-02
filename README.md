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
   - tâche 1 : compter le nomdre de documents par mot --> def compter_nb_doc(corpus):

      - Difficulté : la principale difficulté de cette partie est d’éviter de compter plusieurs fois un même mot dans un document et de conserver la même liste de mots que pour le rôle 2. Sinon, la longueur des listes est différente et il devient impossible d’afficher correctement les résultats.
  
   - tâche 2 : permettre de lister les chemins vers les fichiers du corpus sur l’entrée standard du programme en python :
      ```
        ls Corpus/*.txt | python extraire_lexique.py
      ```
