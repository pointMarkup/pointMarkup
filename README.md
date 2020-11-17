<!--[English translation](readmeEng)-->

# pointMarkup languages `!P:M_L;` PML

**Une autre approche pour baliser un contenu**

***

## _Avant-propos_

Le **but de ce projet** est d'**unifier** un system de balisage utilisable dans diverses situations. <!--Son **parsers** doit être **simple** et rapide.-->  
Actuellement il permet de cataloguer, modeler, compiler et assembler des content-type 'text'. Toute fois il reste en version alpha tant qu'un doute subsiste sur les choix cruciaux qui ont été faits.  

C'est la base de départ à la création d'un ensemble de langages dont la syntaxe est compatibles entre eux. La cohabitation de ces langages dans un même code est ainsi rendu possible. C'est une **syntaxe de balisage** qui utilise grandement les **signes de ponctuations**.  

***

L'origine du **nom pointMarkup** provient qu'il est souvent utilisé dans les **marqueurs d'instructions** le **graphique d'un point** (dot).

***

**Version alpha en cours de rédaction, ne pas utiliser en production ni distribuer!**  

La licence actuelle pour la version alpha est sous CC-BY-NC-ND.  

***

## Objectif

Les deux départs du projet pointMarkup ont été l'inclusion de ressources dans des commentaires de code et de baliser un flux de donnée de content/type 'text'. Depuis les perspectives du projet ont évoluées et permettent d'envisager de multiple applications.  

Sans se perdre dans les détails, on peu s'accorder sur le modèle simplifié d'un langage de programmation suivant:  
>**syntaxe** : normalise l'écriture (reconnaissance)  
> **-> sémantique** : donne l'objectif du langage (action)  
> **-> contenu** : donne le sens (values, data-embedded)  

Ce projet normalise [la base de la syntax](Base)  pour qu'un langage soit compatible **PML** (pointMarkup languages).

Il est prévu des extensions à cette base pour l'adapter aux besoins spécifiques.
<!--
- [Contenu binaire](Base/Extend/Binary).  
- [Librairies pointMarkup](Base/Exend/Library).  
-->
<!--- [Changement de marqueur](Base/Extend/MarkChange).-->

<!--Vous souhaitez d'autres fonctionnalité? Faite une [demandes à intégrer ultérieurement](pointMarkupRequest).-->  

## [Projets compatibles](compatibleProjects)

- **_Petits_**  
Ils utilises la compatibilité **PML** pour développer leur langage et permettent, par leurs expérimentations, de faire évoluer celles-ci afin de dépasser le stade de version alpha.  

  - [markPointPage](https://github.com/pointMarkup/markPointPage) (.mpp)  Simple mise en forme linéaire d'un texte avec hyperliens, étiquette, liste, tableau et image.
  - [rescom](https://github.com/pointMarkup/rescom) (.rsc)  
  Gestion de data-ressources. Ajoute les concepts de variable, fonction, module, template et l'interaction programme avec le résultat du parser.
  - [autres projets ...](compatibleProjects/otherSmall)

- **_Mixés_**  
Ils proposent les langages de plusieurs projets précédents dans un seul projet.  

  - [markpoint](https://github.com/pointMarkup/markPointPage) (.mp)  
  Pour l'instant, il réunit en un seul projet [markPointPage](https://github.com/pointMarkup/markPointPage) et [rescom](https://github.com/pointMarkup/rescom).  
  - [mop](https://github.com/mop) (.mop) modeling output process with interface  in webpage.  

<!--  - [autres projets ...](compatibleProjects/otherMixed)-->

<!--
## [Projets Utilisateurs](compatibleProjects)
-->
