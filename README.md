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
      - Solution : au lieu d'utiliser ma propre façon de séparer les phrase en mot (avant, j'utilise simplement .split()), j'ai recopié celle de role2 en transmettant la liste en l'ensemle qui me permet de résoudre les deux problèmes.
        ```
        r2 : mots = re.findall(r'\w+', texte.lower())  ---> r3 : mots_doc = set(re.findall(r'\w+', doc.lower()))
        set pour le transmettre en l'ensemble qui permet de pas prendre les doublons
        ```
      mais quand je le teste je noté que il y a des mots "chiffrés" (comme 000, 18273, wyeu373684), donc j'ai ajouté une ligne de code pour ignorer les no_alphabet
     ```
     mots_doc = set(re.findall(r'\w+', doc.lower())) 
     mots_doc = [m for m in mots_doc if m.isalpha()] # .isalpha() --> seulement les alphabet peut etre mis dans la liste des mots
     ```
  
   - tâche 2 : permettre de lister les chemins vers les fichiers du corpus sur l’entrée standard du programme en python :
      ```
        ls Corpus/*.txt | python extraire_lexique.py
      ```
