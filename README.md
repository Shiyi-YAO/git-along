# Projet — Lecteur et filtre de flux RSS

Ce projet permet de parcourir l’arborescence d’un dossier contenant des flux RSS et de filtrer les articles selon plusieurs critères :
    - date
    - source
    - catégories

## Getting started

- préparer votre dossier : `chemin_vers_votre_dossier`
- ouvrir votre terminal(c'est mieux d'activer l'environnement virtuel)
- cloner notre repertoitoire et se déplacer dans le projet
    ```
    git clone https://gitlab.com/plurital-ppe2-2026/groupe08/projet.git
    cd projet
    ```
    
- vérifier que vous êtes sur la branche principale
    ```
    git branch
    git checkout main
    ```

- vous pouvez commencer à filtrer votre dossier
    ```
    python rss_reader.py chemin_vers_votre_dossier -d [date_début] -f [date_fin] -s [source] -c [catégories] -cm [mode de match les catégories]
    ```
    ⚠️ Tous les filtres sont optionnels
    - filtrage par date
        - `date_début` et `date_fin` doit etre sous format YYYY-MM-DD (année-mois-date)
    - filtrage par source
        les sources disponibles sont : `blast`, `elucid`, `bfm`, `libération`, `franceinfo`, `lefigaro`
        ⚠️attention aux caractères accentués, si une erreur apparaît
        par exemple : `rss_reader.py: error: argument -s/--source: invalid choice: 'libération' (choose from 'blast', 'elucid', 'bfm', 'libération', 'franceinfo', 'lefigaro')`
        copiez-collez exactement la valeur indiquée dans le message d’erreur afin d’éviter tout problème d’encodage.
    - pour `catégories`, c'est possible d'entrer plusieurs, et vous pouvez choisir le mode de match en utilisant l'option -cm, ici vous pouvez choisi `all` pour tout match ou `any` pour match un des entrées
    voici un exemple de filtres:
    ```
    python rss_reader.py chemin_vers_votre_dossier -d 2025-02-01 -s blast -c culture cinéma -cm all
    ```
    - normalement, après le filtrage, il aura un message qui vous induique des infos sur le filtrage
        - nombre des articles traités
        - nombre des articles trouvés
        - ex: `3658 articles ont été traités, 2 ont été trouvés`
    - votre articles trouvés sera sauvegarder dans un fichier texte `résultat.txt`






