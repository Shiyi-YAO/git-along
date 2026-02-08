# Séance 3

## Membres du groupe
```
r1 : ...
r2 : YAO Shiyi
r3 : ...
```

## Introduction du tâche

Extraire les métadonnées qui serviront au filtrage dans les données fournies au format XML (RSS) :

- l’identifiant (id) de l’article ;
- la source : le nom du journal, qu’on peut approximer avec le nom du fichier ;
- le titre de l’article ;
- le contenu de l’article ;
- la date de l’article ;
- les catégories auxquelles appartient l’article.

Exemple d’extraction à partir d’un fichier XML (RSS):
```
<?xml version='1.0' encoding='UTF-8'?>
<rss xmlns:media="http://search.yahoo.com/mrss/" xmlns:atom="http://www.w3.org/2005/Atom" xmlns:content="http://purl.org/rss/1.0/modules/content/" version="2.0">
  <channel>
   ...
    <item>
      <title>Une histoire politique des musiques noires américaines</title>
      <link>https://www.blast-info.fr/articles/2025/une-histoire-politique-des-musiques-noires-americaines-sMSxOjn2TaaHRPBZiDGBsA</link>
      <description>Blues, jazz, soul, rap : les artistes afro-américains ont de tout temps développé un discours contestataire dans leur musique. Un essai passionnant de Christophe Ylla-Somers : “Le Son de la Révolte”, raconte l’histoire de plus de cent ans de lutte en…</description>
      <guid isPermaLink="true">https://www.blast-info.fr/articles/2025/une-histoire-politique-des-musiques-noires-americaines-sMSxOjn2TaaHRPBZiDGBsA</guid>
      <category domain="https://www.blast-info.fr/tags/culture-zj-sGOhLT-2Gjzuq1CDA5g">Culture</category>
      <category domain="https://www.blast-info.fr/tags/musique-25kaLk3HRgyj7KePfW9lcg">Musique</category>
      <category domain="https://www.blast-info.fr/tags/etats-unis-XypXLrNTQy-h-dxhCfIHXw">États-Unis</category>
      <pubDate>Sun, 09 Feb 2025 11:00:00 +0009</pubDate>
      ...
    </item>
    <item>
    ...
    </item>
```
Les informations dont nous avons besoin se trouvent généralement dans les balises suivantes (qui sont normalement des balises enfants de la balise `<item>`, mais il existe des exceptions, que vous verrez dans l’explication suivante)
```
1. id : <link>...</link>
2. source : Nom de fichier
3. title : <title>...</title>
4. description : <description>...</description>
5. date : <pubDate>...</pubDate>
6. categories : <category>...</category> -> Il peut y avoir plusieurs catégories pour un même article, il faut donc les regrouper dans une liste.
```
Voici un exemple d'extraction:
```
id : https://www.blast-info.fr/articles/2025/une-histoire-politique-des-musiques-noires-americaines-sMSxOjn2TaaHRPBZiDGBsA
source : Blast -- articles.xml
title : Une histoire politique des musiques noires américaines
description : Blues, jazz, soul, rap : les artistes afro-américains ont de tout temps développé un discours contestataire dans leur musique. Un essai passionnant de Christophe Ylla-Somers : “Le Son de la Révolte”, raconte l’histoire de plus de cent ans de lutte en…
date : Sun, 09 Feb 2025 11:00:00 +0009
categories : ['Culture', 'Musique', 'États-Unis']
```

## Les difficulté et solustions

- ### Rôle 1


- ### Rôle 2
   - #### Tâche
     Utiliser le module etree de la librairie standard de python qui permet de lire, modifier et explorer du XML (fonction `lire_rss_etree(chemin_fichier)`)
     
   - #### Difficulté
 
     coming soon
  
   - #### Solutions
  
     coming soon

- ### Rôle 3

## Choix des merges
