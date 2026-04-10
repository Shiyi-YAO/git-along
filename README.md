# RSS Topic Modeling Project
Pipeline complet pour : RSS → corpus → tokens → LDA → topics

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


## Usage (接下来可以体验不同功能, 推荐您一步一步跟随下面的步骤来体验我们所有的功能)

### 1. Lire un flux RSS
C'est une fonction qui vous affiche les infos clé des articles (id, source, title, description, date, categories)
- Pour lire un seul fichier flus rss
    ```
    python rss_reader.py Corpus
    ```
- Pour lire un dossier où il y a plusieurs fichiers flus rss
    ```
    python rss_parcours.py Corpus
    ```

### 2. Construire un corpus_sérialisé (et filtrer)
- Depuis des fichiers RSS et les sauvegarder sous différents format (`xml`, `json`, `pkl`)
    ```
    python rss_parcours.py Corpus -o corpus.[xml, json, pkl]
    ```
    Exemple :
    ```
    python rss_parcours.py Corpus -o corpus.pkl
    ```
- Avec filtres :
    ```
    python rss_parcours.py chemin_vers_votre_doc -d [date_début] -f [date_fin] -s [liste source] -c [liste catégories] -o corpus_sérialisé.[json, xml, pkl]
    ```
    Exemple :
    ```
    python rss_parcours.py chemin_vers_votre_doc -d 2025-02-01 -s blast -c culture -o corpus_sérialisé.pkl
    ```
- Depuis un corpus_sérialisé déjà existant (vous pouvez aussi filtrer et convertir le format) :
    ```
    python rss_parcours.py corpus_sérialisé.[json, xml, pkl] -d [date_début] -f [date_fin] -s [liste source] -c [liste catégories] -o corpus_filtre.[json, xml, pkl]
    ```
    Exemple :
    ```
    python rss_parcours.py corpus_sérialisé.pkl -d 2025-02-01 -s blast -c culture -o corpus_sérialisé.json
    ```

### 3. Ajouter les tokens (analyse)
    ```
    python analyzers.py corpus_sérialisé.[json, xml, pkl] -a [spacy, stanza, trankit] -o corpus_analysé.[json, xml, pkl]
    ```
    Exemple :
    ```
    python analyzers.py corpus_sérialisé.pkl -a spacy -o corpus_analysé.pkl
    ```

### 4. Topic Modeling et Afficher le résultat sous format HTML (有两种Model可以选择, 您可以两种都体验一下然后选择您的偏好)
#### - LDA 
    ```
    python run_lda.py corpus_sérialisé_analysé.pkl -form [lemma, mot] -pos [ADJ, NOUN, VERB...] -o résultat.html
    ```
    Exemple :
    ```
    python run_lda.py corpus_sérialisé_analysé.pkl -form mot -pos NOUN -o résultat_lda.html
    ```
    ⚠️ Ici, n’uploadez pas des fichiers trop petits, sinon il sera impossible de résumer les topics.
#### - BerTopic
    ```
    python run_bertopic.py corpus_sérialisé_analysé.pkl -form [lemma, mot] -pos [ADJ, NOUN, VERB...] -o résultat.html
    ```
    Exemple :
    ```
    python run_bertopic.py corpus_sérialisé_analysé.pkl -form mot -pos NOUN -o résultat_bertopic.html
    ```






