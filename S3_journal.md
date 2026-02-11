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
     Utiliser le module etree de la librairie standard de python qui permet de lire, modifier et explorer du XML (fonction `lire_rss_etree(chemin_fichier)`), l'aventage du module (`etree`) est qu'on peut directement capturer les couches hiérarchies du fichier XML, etree le représenter est avec une structure arbresence. les éléments(méthodes) utilisé pour cette partie
     - `tree = ET.parse('country_data.xml')` : représenter ce fichier avec une structure arborescence
     - `root = tree.getroot()` : récupérer la racine, ici c'est <rss></rss>
     - `root.iter(nom_du_balise)` : parcourir récursivement tous les sous-arbres
     - `Element.find(nom_du_balise)` : récupérer le premier élément avec une balise particulière
     - `Element.findall` : récupérer seulement les éléments avec une balise qui sont les descendants directs de l'élément courant
     - `Element.text` : accèder au contenu textuel de l'élément
     - `if element is not None` : pour éviter les erreurs, car certaines balises peuvent être absentes selon les flux RSS
     
   - #### Difficulté
     - Quand on récupère la description, on note qu'il y pas seulement des texte mais des balises enfants et ils sont transformé sous format html par `CDATA`, c-à-d on perd la structure arborescence ici, on ne peut plus utiliser la façon de etree pour éviter les balises enfant
     ```
     <description><![CDATA[Une nouvelle étude met en avant une rupture culturelle avec les pays du Nord de l'Europe dans l'utilisation de l'argent liquide. Mais si l'usage baisse, la relation aux espèces reste particulière, notamment en France. <br /><br /><img src="https://images.bfmtv.com/kopVULJfD_g_fRYhXbQAgSPnNn4=/4x33:1252x735/800x0/images/-472513.jpg" />]]></description>
     ```
     - Quand on récupère les catégories, je note que pas tous les balise <category> est dans <item> mais certains sont dans <channel>, du coup un simple itération de <item> ne suffit pas, et il y a des cas où le category existe à la fois dans <channel> et <item>, et il peut y avoir des doublons.
       Flux RSS - BFM BUSINESS - Conommation.xml
       ```
       <channel>
         <category>Home Conso - actualités</category>
       <item>
       ...
       </item>
       ```
       Le Figaro - Arts et Expositions.xml
       ```
       <channel>
         <category>Arts Expositions</category>
       <item>
         <category>Arts Expositions</category> ou <category>Culture</category>
       </item>
       ```
     
   - #### Solutions
  
     coming soon

- ### Rôle 3

## Choix des merges
