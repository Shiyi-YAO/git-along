# Rapport — Projet RSS Topic Modeling

## Présentation de l'équipe
### 1. Membres
- CHANFOOK Benoit
- LEE Yeji
- YAO Shiyi

### 2. ...
PluriTAL (Inalco/Sorbonne Nouvelle/Nanterre) - 2526
Projet de Programmation Encadré 2

## Introduction

### 1. Objectif du rapport
Ce rapport a pour objectif d’évaluer le dépôt fourni comme livrable final du projet de Programmation Encadré 2.

Il s’adresse à un destinataire de type « N+1 » : il ne s’agit pas ici d’expliquer en détail comment utiliser l’outil, mais d’examiner dans quelle mesure le livrable répond aux attentes pédagogiques formulées au cours du semestre.

Le rapport s’appuie sur les objectifs rappelés dans les consignes du rendu final. Pour chacun d’entre eux, nous analysons :
- ce qui était attendu ;
- ce qui est effectivement présent dans le dépôt ;
- ce qui fonctionne ou non ;
- l’écart entre l’attendu et l’obtenu ;
- le travail restant pour rendre la fonctionnalité pleinement opérationnelle.

### 2. Présentation synthétique du dépôt
Le dépôt étudié propose une pipeline de traitement de flux RSS allant de la lecture de fichiers XML à la production de visualisations thématiques.

- Le projet s’organise autour des étapes suivantes :
  - lecture d’un ou plusieurs flux RSS ;
  - constitution et filtrage d’un corpus d’articles ;
  - sérialisation du corpus dans différents formats ;
  - analyse linguistique des contenus textuels ;
  - modélisation thématique ;
  - production de visualisations.

- Les principaux scripts identifiés dans le dépôt sont les suivants :
  - `rss_reader.py` : lire un fichier_flux_rss.xml
  - `rss_parcours.py` : lire un fichier ou un dossier RSS, regrouper les articles, les filtrer et exporter le corpus
  - `analyzers.py` : ajouter l’analyse linguistique aux articles avec SpaCy, Stanza ou Trankit
  - `datastructures.py` : définir les classes `Article` et `Token`, et gèrer la lecture/écriture des formats XML, JSON et Pickle
  - `run_lda.py` & `run_bertopic.py` : implémenter la modélisation thématique
