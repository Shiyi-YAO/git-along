# Rapport — Projet RSS Topic Modeling

## Présentation de l'équipe
### - Membres
- `CHANFOOK Benoit` : l’analyse morphosyntaxique et topic modeling LDA
- `LEE Yeji` : La lecture du flux RSS et la (dé)sérialisation
- `YAO Shiyi` : topic modeling BerTopic et la visualisation

### - Cadre académique
PluriTAL (`Inalco`, `Nanterre`, `Sorbonne Nouvelle`) - 2526

Projet de Programmation Encadré 2

## I. Introduction

### 1. Objectif du rapport
Ce rapport a pour objectif d’évaluer le dépôt fourni comme livrable final du projet de Programmation Encadré 2.

Il s’adresse à un destinataire de type « N+1 » : il ne s’agit pas ici d’expliquer en détail comment utiliser l’outil, mais d’examiner dans quelle mesure le livrable répond aux attentes pédagogiques formulées au cours du semestre.

### 2. Méthode d’évaluation
Le rapport s’appuie sur les objectifs rappelés dans les consignes du rendu final. Pour chaque objectif, nous examinons :
- ce qui était attendu ;
- ce qui est effectivement présent dans le dépôt ;
- les principales limites ou écarts observés.

Enfin, nous résumons le niveau de réalisation de la fonctionnalité et le travail qu’il reste à fournir.

### 3. Présentation synthétique du dépôt
- `rss_reader.py` : lire un fichier_flux_rss.xml
- `rss_parcours.py` : lire un fichier ou un dossier RSS, regrouper les articles, les filtrer et exporter le corpus
- `analyzers.py` : ajouter l’analyse linguistique aux articles avec SpaCy, Stanza ou Trankit
- `datastructures.py` : définir les classes `Article` et `Token`, et gèrer la lecture/écriture des formats XML, JSON et Pickle
- `run_lda.py` & `run_bertopic.py` : implémenter la modélisation thématique

## II. Évaluation

### 1. Lire un flux rss unique 

#### - Attendu

#### - Réalisé

#### - Analyse des écarts

### 2. Lire l’arborescence des fichiers et appliquer des filtres 

#### - Attendu

#### - Réalisé

#### - Analyse des écarts

### 3. (Dé)sérialiser les flux rss 

#### - Attendu

#### - Réalisé

#### - Analyse des écarts

### 4. Analyser morphosyntaxique du contenu textuel 

#### - Attendu

#### - Réalisé

#### - Analyse des écarts

### 5. Topic modeling LDA 

#### - Attendu

#### - Réalisé

#### - Analyse des écarts

### 6. Topic modeling BerTopic 

#### - Attendu

#### - Réalisé

#### - Analyse des écarts

### 7. Visualisations

#### - Attendu

#### - Réalisé

#### - Analyse des écarts

## III. Conclusion

### 1. Bilan global

### 2. Travail restant
