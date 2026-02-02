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
    
        au lieu de 看这段命令整体, 我先看左边的部分(`ls Corpus/*.txt`), 因为他的输出将会是我的script的输入, 我在终端lancer这个命令, 我得到了 :
           ```
           
           ```













