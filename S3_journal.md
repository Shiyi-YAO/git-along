# Séance 8

## Membres du groupe
```
Elane Grandmougin : 3(b), 4(b)
Myriam BHS : 3(a), 4(a)
Shiyi YAO : 4(c)
```

## Problèmes rencontrés et Solutions

### Elane Grandmougin

### Myriam BHS

### Shiyi YAO

# Séance 3

## Membres du groupe
```
Elane Grandmougin : 3(b), 4(b)
Myriam BHS : 3(a), 3(b)
Shiyi YAO : 4(c)
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
     Utiliser le module `etree` de python qui permet de lire, modifier et explorer du XML (fonction `lire_rss_etree(chemin_fichier)`), L’avantage du module `etree` est qu’il permet de capturer directement les différentes couches hiérarchiques du fichier XML et de les représenter sous forme d’une structure arborescente.
     
     Les éléments (méthodes) utilisés pour cette partie sont :
     
     - `tree = ET.parse('country_data.xml')` : représenter ce fichier sous forme d’une structure arborescente 
     - `root = tree.getroot()` : récupérer la racine, ici c'est `<rss></rss>`
     - `root.iter(nom_du_balise)` : parcourir récursivement tous les sous-arbres
     - `Element.find(nom_du_balise)` : récupérer le premier élément avec une balise particulière
     - `Element.findall` : récupérer seulement les éléments avec une balise qui sont les descendants directs de l'élément courant
     - `Element.text` : accèder au contenu textuel de l'élément
     - `if element is not None` : pour éviter les erreurs, car certaines balises peuvent être absentes selon les flux RSS
     - Je vous conseille de regarder les utilisation de `etree` à le lien suivant car vous y trouverez des exemples d'utilisation qui permet une compréhension plus intuitive
     - Vous trouverez plus d’informations sur le module etree à ce lien : `https://docs.python.org/fr/3/library/xml.etree.elementtree.html`
     
   - #### Difficulté
     1. Quand on récupère la description, on note qu'il y pas seulement des texte mais des balises enfants et ils sont transformé sous format html par `CDATA`, c-à-d on perd la structure arborescence ici, on ne peut plus utiliser la façon de etree pour éviter les balises enfant
     ```
     <description><![CDATA[Une nouvelle étude met en avant une rupture culturelle avec les pays du Nord de l'Europe dans l'utilisation de l'argent liquide. Mais si l'usage baisse, la relation aux espèces reste particulière, notamment en France. <br /><br /><img src="https://images.bfmtv.com/kopVULJfD_g_fRYhXbQAgSPnNn4=/4x33:1252x735/800x0/images/-472513.jpg" />]]></description>
     ```
     2. Quand on récupère les catégories, je note que toutes les balises `<category>` ne se trouvent pas uniquement dans les `<item>` : certaines sont définies au niveau de `<channel>`, du coup un simple itération de `<item>` ne suffit pas, et il y a des cas où le category existe à la fois dans `<channel>` et `<item>`, et il peut y avoir des doublons.
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
     1. Comme ils sont des `str` et les sous-balises commencent pas `<`, alors on vérifie d'abord s'il y a le sous-balise dans la description et on peut les découper par `<` est prendre que la première élément découpé qui est le contenu qu'on veut pour la description
        ```
        if "<" in description: 
          description = description.split("<")[0].strip() # .strip() est pour enlever les ...
        ```
     2. On récupère aussi les catégories dans `<channel>`(s'il y en a, sinon une liste vide) et celui dans `<item>`(s'il y en a, sinon une liste vide), et on fait le choix entre ces deux liste ou les fusionnent quand le category existe à la fois dans `<channel>` et `<item>`
        ```
        channel = root.find("channel")
        if channel is not None and channel.findall("category") is not None: 
            channel_categories = [c.text for c in channel.findall("category") if c is not None and c.text]
        else:
            channel_categories = []
        ...
        data = {
          ...
          "categories": (
            list(dict.fromkeys(channel_categories + item_categories)) # Si les catégories existent à la fois dans <item> et <channel>, on fusionne les deux listes en supprimant les doublons
            if len(item_categories) > 0 and len(channel_categories) > 0
            else item_categories if len(item_categories) > 0 # Sinon, s’il n’y a que des catégories dans <item>, on utilise item_categories
            else channel_categories # Sinon, on utilise channel_categories
          )}
        ```
   - #### Corrections
     Après avoir comparé la correction fourni par les profs et tester mes codes sur l'ensemble des fichiers, j'ai constaté un problème : la méthode basée sur xml.etree.ElementTree ne permet pas de traiter correctement tous les fichiers XML. Lors de l’exécution, les erreurs m'indiquent que certains fichiers XML ne sont pas valides ou ne respectent pas strictement la structure attendue. Afin de rendre le programme exécutable, j'ajoute un bloc `try / except` qui afficher un message qui indique les fichiers non valides, et le programme continue son exécution en renvoyant une liste vide pour le fichier concerné.
     ```
     # pour traiter les fichier XML non valide
     try:
       root = ET.parse(chemin_fichier)
     except ET.ParseError:
       print(f"Fichier {chemin_fichier} invalide.", file=sys.stderr)
       return []
     ```
     Et j'ai aussi corrigé la condition pour la description pour éviter des cas où ("description").text est None
     ```
     if "<" in description:
             ↓
     if description and "<" in description:
     ```
        
- ### Rôle 3

## Améliorations

- ### Rôle 1
  (Lecteur `r3` : ...)


- ### Rôle 2
  (Lecteur `r1` : CHALABI Sara Amina)


- ### Rôle 3
  (Lecteur `r2` : YAO Shiyi)
   - #### Problèmes
     En lisant les code de rôle 3 qui utilise `feedparser`, j’ai constaté que certaines difficultés observées en tant que rôle 2, notamment concernant les balises `<description>` et `<category>`, n’avaient pas été prises en compte (voir la description détaillée des difficultés dans la partie `Difficultés` du rôle 2).
   - #### Solutions
     Ayant déjà identifié ces problèmes lors de la réalisation de ma propre partie, j’ai appliqué une logique similaire pour modifier le code du rôle 3. Cela permet de garantir que les résultats finaux obtenus soient le même. Mais je suis pas très familier avec l’utilisation de `feedparser`, ça m'a pris un peu de temps d’étudier son fonctionnement --> ce module permet de parser facilement des flux RSS sans avoir à manipuler directement la structure XML, il extrait automatiquement les informations principales d’un flux RSS (titre, lien, description, date de publication, identifiant, catégories, etc.) et les met à disposition sous forme de structures Python simples (dictionnaires et listes).

     Voici les méthodes j’ai appris et utilisés : :
       - `feed.feed` : accessible aux éléments du canal (title, link, description, pubdate ... )
       - `feed.entries` : accessible aux articles (items/entries)
       - Je vous conseille de regarder les utilisation de `feedparser` à le lien suivant car vous y trouverez des exemples d'utilisation qui permet une compréhension plus intuitive
       - Vous trouverez plus d’informations sur le module etree à ce lien : `https://feedparser.readthedocs.io/en/latest/common-rss-elements/`
     
     Cela pourrait vous intéresser : Quand vous lisez les codes, vous verrez des parties où il y `getattr`, c'est une façon que rôle 3 utilise pour réaliser `getattr(obj, "nom de l'attribut", valeur par défaut)` qui permet de récupèrer le contenu de `nom de balise` ; s'il n'existe pas, elle renvoie la valeur par défaut.

## Choix des merges
Dans la dernière partie, on n'a pas vraiment eu à faire de choix de merge, car dès le début on est tous partis du squelette sur la branche `dev` fourni par r1 pour développer nos propres parties de code. 

Ce squelette proposait déjà :
  - une fonction de combinaison permettant de fusionner les différentes méthodes développées par les trois membres du groupe (Après avoir configuré le parser, il suffit d’appeler la fonction combinée lire_rss en lui passant le chemin du fichier RSS ainsi que la méthode de lecture souhaitée)
    ```
    def lire_rss(chemin_fichier, methode="regex"):

      if methode == "regex":
          return lire_rss_regex(chemin_fichier)
      elif methode == "etree":
          return lire_rss_etree(chemin_fichier)
      elif methode == "feedparser":
          return lire_rss_feedparser(chemin_fichier)
      else:
          raise ValueError(f"Méthode inconnue : {methode}")
    ```
  - une fonction main chargée de configurer le parser et d’afficher les résultats.
    ```
    def main():
      parser = argparse.ArgumentParser(description="Lecteur de flux RSS")
    
      parser.add_argument(
        "fichier",
        help="Chemin vers un fichier RSS XML"
      )
    
      parser.add_argument(
        "-m", "--methode",
        choices=["regex", "etree", "feedparser"],
        default="regex",
        help="Méthode d'extraction (défaut : regex)"
      )
    
      args = parser.parse_args()
    
      articles = lire_rss(args.fichier, args.methode) # l'appelle de la fonction de choisir le méthode ici
    
      for article in articles:
        print(article)
    
    
    if __name__ == "__main__":
      main()
    ```
Pour l'affichage finale, nous avons appliqué la façon de r2 qui consiste à parcourir la liste des dictionnaires , où chaque clé correspond à un attribut et chaque valeur au contenu associé (par exemple : `{id: https... ; source : Flux RSS... ; ...}`)

    ```
    for article in articles[:1]: # vous pouvez modifier le chiffre dans [] pour controler le nombre d'article affiché
      for k, v in article.items():
        print(f"{k} : {v}")
      print() # une ligne entre les articles
    ```






