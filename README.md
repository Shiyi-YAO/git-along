# RSS Topic Modeling Project
Pipeline complet pour :
- RSS → corpus → tokens → LDA → topics
- lire des flux RSS
- filtrer des articles
- ajouter des tokens (NLP)
- faire du topic modeling (LDA)

⚠️ vous trouverez dans chaque fichier .py des exemples de commandes dans `main` pour exécuter le fichier

## Pour commencer (跟随下面的步骤)
1. Ouvrir votre terminal(⚠️activer l'environnement virtuel)
2. Cloner notre repertoitoire et se déplacer dans le projet
    ```
    git clone https://gitlab.com/plurital-ppe2-2026/groupe11/projet.git
    ```
    ```
    cd projet
    ```
3. Préparer votre ficheir ou dossier de flux RSS et le nommer `Corpus` pour vous puissse utiliser directement les commandes suivants
4. Installer les outils : (⚠️ Vous n’avez besoin d’installer que l’outil d’analyse que vous allez utiliser. Nous recommandons SpaCy)
    - SpaCy : 
    ```
    pip install spacy
    python -m spacy download fr_core_news_sm
    ```
    - Stanza :
    ```
    pip install stanza
    ```
    - Trankit : (il faut python 3.10)
    ```
    uv pip install https://github.com/pmagistry/trankit.git
    ```


## Usage

### - Lire un flux RSS (un seul fichier)
- Affiche les articles (id, source, title, description, date, categories)
    ```
    python rss_reader.py chemin_vers_votre_doc
    ```

### - Construire un corpus_sérialisé (et filtrer)
- Depuis des fichiers RSS :
    ```
    python rss_parcours.py chemin_vers_votre_doc -o corpus.[json, xml, pkl]
    ```
- Avec filtres :
    ```
    python rss_parcours.py chemin_vers_votre_doc -d [date_début] -f [date_fin] -s [liste source] -c [liste catégories] -o corpus_sérialisé.[json, xml, pkl]
    
    exemple : python rss_parcours.py chemin_vers_votre_doc -d 2025-02-01 -s blast -c culture -o corpus_sérialisé.[json, xml, pkl]
    ```
- Depuis un corpus_sérialisé déjà existant (vous pouvez aussi filtrer et convertir le format) :
    ```
    python rss_parcours.py corpus_sérialisé.[json, xml, pkl] -d [date_début] -f [date_fin] -s [liste source] -c [liste catégories] -o corpus_filtre.[json, xml, pkl]
    ```

### - Ajouter les tokens (analyse)
```
python analyzers.py corpus_sérialisé.[json, xml, pkl] -a [spacy, stanza, trankit] -o corpus_analysé.[json, xml, pkl]
```

### - Topic Modeling (LDA) et Afficher le résultat sous format HTML
```
python run_lda.py corpus_sérialisé_analysé.pkl -form [lemma, mot] -pos [ADJ, NOUN, VERB...] -o résultat.html
```
⚠️ Ici, n’uploadez pas des fichiers trop petits, sinon il sera impossible de résumer les topics.






