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
   - tâche 1 : Écrire une fonction prenant comme argument une liste de chaînes et retournant un dictionnaire associant chaque mot au nombre de documents dans lequel il apparaît.
      ```
      def compter_nb_doc(corpus):
         freq_doc = {} # initialiser un dictionnaire -> {mot : nombre de documents dans lequel il apparaît}
   
         for doc in corpus: #utiliser la meme façon que r2 pour avoir les meme mots
            mots_doc = set(re.findall(r'\w+', doc.lower())) #ici, il doit etre un ensemble pour ne pas avoir des mots doublons
            mots_doc = [m for m in mots_doc if m.isalpha()]
            for mot in mots_doc:
               if mot in freq_doc: #s'il est dans ce doc, ajouter 1
                  freq_doc[mot] = freq_doc[mot] + 1
               else:
                  freq_doc[mot] = 1
   
          return freq_doc
      ```
   - tâche 2 : permettre de lister les chemins vers les fichiers du corpus sur l’entrée standard du programme en python :
      ```
        ls Corpus/*.txt | python extraire_lexique.py
      ```
