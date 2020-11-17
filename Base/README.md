<a>[Retour](../)</a>
<hr style="height:8px; background-color:rgb(209, 87, 112);border:0px">

# Base de la syntaxe d'un pointMarkup `!:_;`

**Tronc commun pour un langage d'instructions embarquées compatible pointMarkup.**

***

## _Le nom pointMarkup_  

La plupart des principaux identifiants (!, ?, :, ;, ...) contiennent un point (dot) d'où le nom pointMarkup.  

***

**Version alpha en cours de rédaction, ne pas utiliser ni distribuer!**

La licence actuelle pour la version alpha est sous CC-BY-NC-ND.  

<a id="docIndex"></a>
<hr style="height:8px; background-color:rgb(209, 87, 112);border:0px">

## _Index_

<hr style="height:4px; background-color:rgb(209, 87, 112);border:0px">


## _Avant-propos_

Cette syntaxe permet d'intégrer une dimension modulaire à tous fichier texte ou de ressource. Elle permet aussi la génération de 

Initialement écrit en français, ce document utilise l'anglais pour définir les termes employés comme référence.  
Ce document possède une [version technique qui fait fait office de référence](technicalReference).  

A l'origine le projet pointMarkup à été prévu pour baliser un flux de donnée de content/type 'text' puis de permettre l'accès aux données et méthodes contenues dans un pointMarkup par un système de persistance après l'interprétation d'un pointMarkup. Son objectif d'origine est donc axé sur la génération de document et de ressources.  
Maintenant il est devenu une surcouche, de gestion et de traitement, d'un flux de data.  

    pointMarkup =  
    ...datalow...<startMarkup>...dataFlow...<endMarkup>...dataFlow...  

Vu coté organisation du protocole de codage, il y a très peu de différence entre un langage à balises et celui par instructions. Les instruction sont plus adaptées aux traitements et les balises à l'organisation. L'écriture de texte semble plus simple avec les balise et il ne peut y avoir rien d'autre que des instructions en dehors d'une instruction. Pour pouvoir utiliser pointMarkup dans divers situations il a été décidé de faire un mixte entre balise et instruction ce qui ce résume à des **instructions embarquées** ([\<embeddedInstructions>](#dtEmbInst)).  

**`...aData...<embeddedInstructions>...dataTxt...`**  

>**Dans la syntaxe pointMarkup les donnée peuvent embarquer des instructions et les instructions des données.**  

A première vue c'est similaire dans le principe à HTML mais en fait c'est plus proche de PHP à deux différences:  

1) La méthode de basculement entre fluxText et code d'instruction.  
2) La persistance des données et méthodes.  

<!--- L'absence d'instructions de programmation standard (arithmétique, logique, boucle) dans la syntaxe de base d'un pointMarkup ([plus de détails ici](#methodes)).-->

<!--Pour la base de la syntaxe d'un pointMarkup il ne faut pas s'attendre à une programmation sophistiqué.-->  

**_Cette syntaxe s'attache à faciliter une conception modulaire fonctionnelle et mettre en avant la finalité._**  

### Un pointMarkup

**Un pointMarkup** est une **data-texte** qui peut être **décodé** selon la syntaxe pointMarkup. Dans le principe, les **instructions embarquées** ( si il y en a ) sont **remplacées** par le résultat de celles-ci ou rien.  

Il peut être placées dans une variable texte, un commentaire de script (en javascript par exemple) ou un fichier texte externe.  

Un pointMarkup sans instructions embarquées est une donnée content-type 'text'. Ceci implique que tout fichier texte est compatible pointMarkup s'il ne comporte pas de [conflit](#conflictInst) possible avec les instructions.  

**_Les échanges utilisateurs:_**  
Le décodage d'un pointMarkup permet l'**interaction** avec [l'**utilisateur final**](#exchangeUser) et l'accès en interne à [l'espace de nom du pointMarkup](#nameSpace).  

### **_Caractère blanc:_**  

**`espace, \t, \r, \n (\r\n)`**  

- L'espace, la tabulation, le retour chariot et le saut de ligne.  
- Le début et la fin d'un pointMarkup sont considérés comme un caractère blanc.

### **_Note de rédaction:_**  

Toute ligne ou bloc de lignes commençant par le caractère:  

- **?** est à confirmer (suggest, alpha).  
- **~** est à préciser (beta).  
- **&** tilde(s) est une réponse ou discussion.  

si suivi d'un espace ou du nom de l'auteur suivi d'un espace.  
**`<(?|~|&)*>[<authorName>]< >`**  

le nombre de répétition (**\***) détermine la profondeur de l'imbrication.  

La licence actuelle pour la version alpha est sous CC-BY-NC-ND.  

<br><hr style="height:8px; background-color:rgb(209, 87, 112);border:0px">
<span id="dtEmbInst" style="float: right;">[Retour index](#docIndex)</span>

## \<embeddedInstructions>

Forme générique utilisée par le [projet pointMarkup](https://github.com/pointMarkup/pointMarkup).  

**`[<aData>][< >]<pointCommands>[<immediateAData>][< ><inlineAData>\n][< >][<aData>][< >][<pointCommands>]...`**  

Un pointMarkup peut être codé en Unicode mais **`<pointCommands>`** doit pouvoir être codé en ASCII simple.  

**`<pointCommands>`**, parfois appelé commande ou instruction dans la rédaction, représente le couple indissociable **`<markers><commands>`** sensible à la casse.  
Un **\<pointCommands> non valide** est **conservé dans l'état** et réintégré à aData. Attention à sa commande de fin (si elle existe \<commandEnd>) qui peut, **elle**, être valide.  

>**_Note:_**  
> **!** une marque de commentaire (d'autre langage) ou tout caractère blanc avant ou après un \<pointCommands> fait l'objet de [règles particulières](#CommentWhite).  

<br><hr style="height:4px; background-color:rgb(209, 87, 112);border:0px">

### (_\<markers>_) `!`

Pour la base il a été retenu, un **unique marquer**, le **point d'exclamation** ( **`!`** ). **L'utilisation d'autre marquer est réservé** au projet pointMarkup et sort de la Base de la syntaxe d'un pointMarkup.  

Le présent document traite de la base de la syntaxe qui utilise le marqueur point d'exclamation. Par soucis de compréhension \<markers> sera souvent remplacé par **`!`** dans la suite de la rédaction mais n'en reste pas moins équivalent à \<markers> pour un langage compatible pointMarkup.

Toute **\<commands>** est donc **précédée** d'un \<marker> point d'exclamation ( **`!`** ).  

#### Conflit d'interprétation `!` avec \<aData> <a name="conflictInst"></a>

- <a name="escapeInst"></a>Pour **échapper une commande** (ne pas l'interpréter) ou ce qui pourrait y ressembler, on ajoute une **quote inversée** entre le **`!`** et la commande: **`!`\<commands>`**  
- **_Faux semblant_**  
  - Lorsque une donnée représentent le **sources d'un programme** il est fréquent d'utiliser **`!`** pour la négation ( **not** ) dans ce cas **ajouter un espace** après cette négation si cela est possible.  
  - En second recours il est possible d'utiliser la méthode de l'[échappement de commande **`!'`**](#escapeInst).  
  - Dans d'**autres situations** il est **impossible** de **transformer** la donnée. Exemple dans certaine sources **`!=`**, en html **`<!--`**, les images en markdown **`![`**, le gras markdown  `**data...!**` ou plus insidieux une donnée se terminant par un '!' (ex: `"data...!"`, `data...!<...>`) ...  
  Dans ces cas on utilise l'[interruption momentanée de l'interprétation d'instructions](#pauseInst) **`!~[<number>]: <aData>!~[<number>];`**.  
  
<br><hr style="height:4px; background-color:rgb(209, 87, 112);border:0px">

### **_Specific form of embedded instructions_**

- **auto-close :**  
  - no data, return **constant text** or void  
    **`!<command>`**  
  - no data  
    **`!<command>`**  
  - immédiate data ( underscore remplacé par espace )  
    jusqu'au premier caractère blanc.  
    **`!<command><immediateAData>< ><aData>`**  
  - by first \n or \<command(no constant text)> into data  
    **`!<command>< ><inlineAData>\n<aData>`**  
  - by first doubles \n  or \<command(no constant text)> into data  
    **`!<command>< ><inlineAData>\n\n<aData>`** 
  - by other \<embeddedInstructions>:  
    **`!<commands>[< ><aData>]...<someEmbeddedInstructions>`**
  - by identic commands :  
    **`!<commands>[< ><aData>]...!<identicCommands>`**  
- **single data :**  
**`!<commands>...<aData>...!<commandEnd>`**  
- **multi-data:**  
**`!<commandStart>< ><aData>(!<commandEnd[,|:]>< ><aData>)*...!<commandEnd>`**  
Note : `<commandEnd[,|:]` est souvent écrit `<commandNext>`    
- **closing required :**  
  \<commandEnd> peut être obligatoire sur **single data** ou **multi-data**  

Note: les \<embeddedInstructions> utilisant une \<commandEnd> ne peuvent s'imbriquer.  

<br><hr style="height:4px; background-color:rgb(209, 87, 112);border:0px">

### _\<commands>_  

**\<commands>** liée à \<markers> représente **une instruction**.  

**Forme _simplifiée_ de la base de la syntaxe d'un pointMarkup**  
**`[<aData>][< >]!<commands>[<immediateAData>< >|< ><inlineAData>\n|< >]<aData>]...`**  

Une commande peut être une ouverture, une fermeture ou une simple instruction sans fermeture.

**Un pointMarkup** est obligatoirement cohérent ce qui implique que la première commande rencontrée ne peut être de fermeture et que toutes commandes ouverte et non fermée seront fermées au moment opportun par le parser. S'il existe des fermetures orphelines en début du pointMarkup elles seront ignorées et remplacées par void.  

**_Particularité de traitement d'une commande_**  
Aux formes que peut prendre une commande s'ajoute une particularité d'utilisation :  

- **normal**  
- **obligation de fermeture**  
Indique que la fermeture est obligatoire et non simplifiable.  
- **de fermeture**  
Permet de fermer une commande précédemment ouverte.  
- **de séquence**  
Permet d'organiser une suite de commandes en liste organisée ou non.  
- **non réentrant**  
- **de saut ou sortie de séquence**  
Permet de ce déplacer dans une séquence ou d'en sortir.  
- **liaison**  
Permet de passer des informations d'une commande à une commande précédente.  

_Le format d'une commande est [détaillé après \<aData>](#ddCommands)_  
_Le format des fermetures dite \<commandSub> et  \<commandEnd> est [détaillé à la fin de celui des commandes](#ddCommandsSubEnd)_  

<br><hr style="height:4px; background-color:rgb(209, 87, 112);border:0px">

### _\<aData>_

L'utilisation et l'interprétation de \<aData> est laissé à la charge du language.  

#### \<aData> en format binaire

L'instruction de [pause temporaire](#pauseInst) permet d'embarquer du contenu binaire.


#### \<aData> en format Texte  

- c'est potentiellement un pointMarkup.  
- et peut représenter n'importe quoi même du code.  
- \<aData> est tout ou partie d'une data.  
- Peut **être** une ou des **instructions embarquées**.
- Peut **contenir** des **instructions embarquées**.  
- Sauf exception la séquence commande donnée est toujours séparée par un caractère blanc non inclus dans la donnée.
- Si la donnée commence par une instruction embarquées le caractère blanc de séparation peut-être omis.  
- Si la séquence commande donnée est séparée par au moins un caractère blanc jusqu'à un saut de ligne. Ceux-ci ne font pas parties de la donnée et implique un [traitement particulier](#cmdBlancData).
- La séquence donnée instruction peut ne pas être séparée par un caractère blanc.  

<br><hr style="height:8px; background-color:rgb(209, 87, 112);border:0px">
<span id="dtEmbInst" style="float: right;">[Retour index](#docIndex)</span>

## <a name="ddCommands"> \<commands></a>  

Une commande normale s'écrit :  
**`[<target>][:][<actionOptions>][<property>][(<smartArea>)][{<freeArea>}]`**  
Une commandeEnd est une fermeture de commande et s'écrit :
**`[<target>][<property>][;[<subValue>]][(<smartArea>)][{<freeArea>}]`**  

excepté pour la fermeture d'une instruction **`!;`**, \<property> sans \<target> ou \<actionOptions> n'a aucun sens.  

- Une commande fait **au moins un caractère et ne peut contenir de caractère blanc** (excepté dans smartArea et freeArea) ni de \<markers> (en l'occurrence **!**).  
- Une commande peut **contenir des underscores** mais ceux-ci seront **généralement ignorés** ([voir traitement underscore](#ttmUnderscore)).  
- Toute séquence **`*/`** en fin de commands est ignorée et perdue. Ailleurs cette séquence est interdite.  
- Toute \<commands> non fermante est rattaché à une \<aData> qui la suit sauf pour. 
- A l'exception de sa <aData> une **instruction** doit pouvoir être **codée** en **ASCII** non étendue bien que cela ne soit pas imposé.  
- Une **instruction valide** contient au moins une commande reconnue.  
- **_\<aData>_** peut être vide.  
- **_!\<commandsEnd>_** n'a pas de \<aData> et n'est **pas obligatoire** c'est le cas pour l'auto-close. D'autres instructions peuvent accepter à la place un ou deux saut de ligne ou le début de certaines instructions ( close by ou l'enchaînement des commandsNext ).  

### <a id="motifInst"> motif-instruction</a>  

Ce concept est utilisé pour désigné la suite **`<target><property>`**.  
Cette suite indique l'instruction, ou un groupe de \<commands> dont l'instruction est ciblée par <actionOptions>. C'est aussi la suite utilisé pour une \<commandEnd> ou \<commandSub>.  

PointMarkup permet de simplifier l'écriture d'une instruction grace à \<target> et \<property>. C'est une sorte de raccourci qui doit resté mnémonique et si possible visuellement.  
Il est donc recommandé d'avoir une certaine **cohérence** dans le **choix des motifs** afin de ne pas ce perdre dans ce qui peut vite devenir des signes cabalistiques. De même, la longueur d'un motif doit être inversement proportionnelle à utilisation de l'instruction (court = fréquente) est une bonne contrainte à adopter.  
Le premier et dernier caractères de \<target> et \<property> sont généralement déterminant mais pas illimités. Afin de laisser de la place à d'autres langages compatible pointMarkup, il est aussi demandé de ce limiter dans la diversité de leurs utilisations.  

### _\<target>_ (prefix commands)

Suite de caractères spéciaux ( virgule exclu ) %, :, ?, ., `, ", *, =, #, ^, \, /, -, ~, <, > et |.  

- Peut se terminer par un Underscore **( _ )** si non suivi d'un autre Underscore.  
- Ne doit pas se terminer par les caractères  :, %, ? la virgule et le point (.).  

### _\<actionOptions>_

Se décompose ainsi :  
\<action> suivi de une ou plusieurs \<options>  
Le concept \<action> est utilisé pour désigné la partie déterminante (si elle existe) de \<actionOptions>.  

- Sensible à la casse.  
- Ne peut contenir de caractère blanc.  
- Contient au moins deux caractères (pour les noms utilisateur).  
- Commence par **:** et/ou un caractère alphabétique (underscore et tiret inclus).  
- Si commence par underscore, le premiers appartient à \<target>.  
- Se termine par un caractère alphanumérique (underscore et tiret inclus).  


### _\<property>_ (suffix commands)

Suite de caractères spéciaux.  

### (\<smartArea>) et {\<freeArea>}  

Utilisés par l'interpréteur d'instruction, commencent et ce termine par un caractère blanc.  

- smartArea ne peut contenir un caractère blanc suivi de **')'**.  
- freeArea ne peut contenir un caractère blanc suivi de **'}'**.  
  
<br><hr style="height:4px; background-color:rgb(209, 87, 112);border:0px">

### _Les fermetures_<a name="ddCommandsSubEnd"></a>

Regroupe les sous instructions **\<commandsSub>** et les fin d'instruction(s) **\<commandsEnd>**.  

**`!` [ `<startMotif>` ] `;` [`^|:|#`][`(<smartArea>)`][`{<freeArea>}`][`_`]**  

- **`!;`** ferme la dernière instruction ouverte et pas encore fermée.  
- **`!<startMotif>;`** ferme l'instruction ouverte dont le début ou la totalité de son [motif-instruction](#motifInst) correspond à \<startMotif>. En dehors des instructions auto-close le fait de fermer une instruction précise ferme automatiquement toutes instructions contenues non fermées.  
- sans [`^|:|#`] fin d'instruction sinon :   
**`:`** fin de sous instruction (souvent en haut "header")  
**`^`** suite de sous instruction  
**`#`** début de sous instruction (souvent en bas "footer")
- Si besoin **`[(<smartArea>)][{<freeArea>}]`** peuvent être indiquées après tout **`;`** de fermeture même s'il en existe déjà  dans l'ouverture de l'instruction à fermer (en cas de conflit  l'ouverture est prioritaire à la fermeture).  
- **`_`** n'a pas de signification particulière,  il permet d'éviter les conflits avec ce qui suit une fermeture. Exemple avec aData commençant par ^, :, #, [, (, {, ou _.  

Une fermeture :  

- ferme toutes instructions non fermées et contenues dans l'instruction ou sous-instruction lié à cette fermeture.  
- ne peut intervenir dans une instruction parent ou enfant.  
- n'est pas suivie d'un espace obligatoire sauf pour \<commandsSub>  
- \<commandsSub> qui ne ce termine pas par **`#`** possède un  aData qui la précède  
- \<commandsEnd> non précédée d'une \<commandsSub> et toute \<commandsSub> qui ce termine par **`#`** n'ont pas de  \<aData>.

Note : Pour simplifier la lecture il est généralement écrit **`!;`**,**`!;^`**,**`!;:`** et **`!;#`** dans ce document pour une fermeture. Mais cela inclus la possibilité d'utiliser **`<startMotif>`**, **`<smartArea>`** et **`<freeArea>`**.

<!--
***

### regExp  

  <pre>
  Avec  
  marker = /!(?![\())\s\[!,_=\\-])/  
  target = /(`?\\.?["*=#^%\\\\\\/\\-~<>|]*_?)/  
  actionsName = /(:*\\.?[\\w\\-\\&]*)((?:[#@\\.=:]\\w+)*(?:_(?!_))?)/  
  property = /([\\.\?]?["*=#^%\\\\\\/\\-~<>|\\-:|]*(?:_(?!_))?)(;)?/  
  smartArea = /(?:\\(((?:.?\\n*)+)\\))?/  
  freeArea = /(?:{((?:.?\\n*)+)})?/  
  et endSpace = /([\\s_]?)/,  
  l'expression rationnelle :  
  /!(?![\())\s\[!,_=\\-])(`?\\.?["*=#^%\\\\\\/\\-~<>|]*_?)(:*\\.?[\\w\\-\\&]*)((?:[#@\\.=:]\\w+)*(?:_(?!_))?)([\\.\?]?["*=#^%\\\\\\/\\-~<>|\\-:|]*(?:_(?!_))?)(;)?(?:\\(((?:.?\\n*)+)\\))?(?:{((?:.?\\n*)+)})?([\\s_]?)/g  
  permet d'isoler (sans valider)  
  target ($2),  
  cmdName ($3),  
  param ($4),  
  property ($5),  
  endCommand ($6),  
  (...smartArea...) ($7),  
  {...freeArea...} ($8),  
  endSpace ($9),  
  et par déduction motif-instruction ($2+$4).
  </pre>

***
-->

<br><hr style="height:8px; background-color:rgb(209, 87, 112);border:0px">
<span id="dtEmbInst" style="float: right;">[Retour index](#docIndex)</span>

## Common commands  

### **_Choix de \<motif>_**  

Règles utilisées pour un \<motif> :  

- En premier caractère  
  - **`/`** pour les commentaires.  
  - <code>**`**</code> pour échapper une instruction.  
  - **`~`** pour system parser.  
  - **`.`** pour constante, template et macros.  

...
### Commentaires

Les commentaires pointMarkup sont orienté de facto documentation et fonctionne différemment d'autres langage.  
Ils n'ont pas vocation à pouvoir désactiver une portion de code utilisez la [pause d'interprétation d'instructions](#pauseInst) pour cela.  
Ils peuvent agir sur le flux out et l'espace de nom comment.  

_**Sous commentaires :**_  
Généralement réservés aux commentaires de programmation.  
Ils sont de format texte et les instructions pointMarkup ne sont pas interprétées.  

- de ligne/mot (\<word>) : **`!/[< >|<label>]`** de ligne (jusqu'à \n ou fin de mot \<label> si collé.  
- de bloc : **`!//[<label>] ...<commandEnd>`**

_**Commentaires avec instruction pointMarkup :**_  
Pour informer globalement du but, des techniques et méthodes utilisées. Ils sont de format html et les instructions pointMarkup sont interprétées.  

- de ligne/mot (\<word>) : **`!/<label>:`** de ligne (jusqu'à \n) ou de mot si collé.  
- de bloc : **`!//<label>: ...commentArea...[!;:]...data...<commandEnd>`**

- **~** L'instruction **`!/*/`** sert uniquement à clôturer un bloc de commentaire d'un autre langage et fait généralement suite à **`/*!...`** en amont. Elle est remplacée par **`*/`** pour le flux **echo** sinon par 'void'.
- **~** L'instruction **`!/`** (suivie obligatoirement d'un espace) est remplacé par un simple slash (**`/`**). Utilisée pour l'écriture d'un pointMarkup dans un commentaire de bloc (/*...commentCode.. \*!/ dans pointMarkup...\*/). Elle  permet (en cas d'imbrication de commentaire) d'échapper l'écriture d'un fin de bloc de commentaire d'un autre langage. Ex:**`*!/`** équivaut à **`*/`** après décodage.  

### Escape instruction

Pour convenir à de multiples situations, pointMarkup est riches en échappement d'instructions.  

#### En place

**<code>!\`\<commands>...</code>** remplace l'instruction, valide ou non, par le texte **`!\<commands>...`** ce qui équivaut à ne pas interpréter l'instruction.  

#### underscore  

**`!_`** pour empêcher le parser de remplacer underscore par un espace.  

#### Pause temporaire

<!--**<code>!~\`\<commands>...</code>** n'exécute pas la commande, mais la conserve, si la sortie est la génération d'un pointMarkup.-->  
**`!~[<motif>|\<number>]: <aData>!~[<motif>|\<number>];`**  
Suspend l'interprétation d'instructions de  
**`!~[<motif>|\<number>]:`** à **`!~[<motif>|\<number>];`**  
Voir [pause interprétation d'instructions](#pauseInst)).  

<!--#### \<markers> alternatif

Pour éviter certains conflits:  
Utilisation de **`!-<commands>...`** à la place de **`!<commands>...`**.  
L'escape en place s'écrit ainsi:
**<code>!\`-\<commands>...</code>** <!---->

<!--#### ? Deux ou trois \<markers>

? Utilisation temporaire de [deux trois ou quatre \<markers>](#overMarkers)  
? debut **`!![![!]]~ ...`** **`!![![!]]...`** **`!![![!]] ...`** en fin **`!![![!]]~;`** <!---->

<!--TODO: 2 ou 4 seulement  
Dans ces cas on **ajoute autant de \<markers>** que nécessaire **dans le \<markers>** d'instruction avant le conflit et les \<markers> d'instructions suivantes.  
Pour **arrêter cette surpopulation du \<markers>** on ajoute un '**!**' supplémentaire dans le \<markers> d'instruction après le conflit avant de revenir à l'écriture normale d'un seul '**!**'  
-->  

### constant  

**`!..<constantName> aData`**  
S'utilise **`.constantName`** à la fin de \<actionOptions>  
Et **`!constantName.`** dans aData  


### template  

#### de type simple  

Constantes avec variables intégrées  
param : !$[n]  
Pour un comportement embInst  
aData : !$ (par défaut)  
freeArea : !$f  
action : !$a  
Pour accepter  un tableaux de variable à deux dimension et donc itérer lors de l'utilisation : !% indique le début d'une itération possible  
nom de variable : !$\<name> avec name d'au moins 2 caractères.  
~ **`!..<templateName> aData...[!<n>.] aData...`**  
S'utilise **`.templateName,nData...`** à la fin de  \<actionOptions> ou entre {...}  
Et **`!templateName.< >nData !, n+1Data...!;** dans aData  

#### de type liste  

fonctionne comme une liste  
**`!.[ ...`** mixte tableaux dictionnaire  
**`!.* ...`** mixte tableaux dictionnaire  

### <a name="supCmdEnd" >Suppression de \<commandsEnd> </a>  

Si **aData** ne contient **pas de caractères blanc** \<commandsEnd> n'est pas requis.  
Dans ce cas il n'y a **pas d'espace entre \<commands> et \<aData>**.  
Et tout **underscore** dans aData est **remplacé par un espace** à l'exception du premier qui est ignoré.  

### trim aData

Si ils existe retire le retour à la ligne en début et en fin de aData.  
Ensuite si le nombre de tabulations en début de première ligne est supérieur ou égale aux nombre de tabulation avant l'instruction, tous les début de lignes ayant un nombre nombre de tabulations égale ou supérieur ce voient retirée ce nombre de tabulation.  

***

### <a name="parserInst"></a>Système parseur

#### Void

? **`!~`**  
~ **`!..`**  
Remplacée par rien.  
<!--Peut aussi être utilisé pour arrêter ou démarrer la [surpopulation](#overMarkers) de \<markers>.-->  

#### <a name="pauseInst">Pause interprétation d'instructions</a>

**?** Suspend l'interprétation de certains motifs  
Suspend l'interprétation d'instructions  
**`!~[<motif>|\<number>]: <aData>!~[<motif>|\<number>];`**  
de **`!~[<motif>|\<number>]:`** à **`!~[<motif>|\<number>];`** (syntaxe de fin obligatoire)  

**Avec \<motif>**  
? motif contenu valide à préciser.  
**`!~<motif>: <aData>!~<motif>;`** ne pas interpréter les instructions que pourrait contenir aData jusqu'à **`!~<motif>;`**(les deux \<motif> doivent être identique).  
**Sans \<motif>|\<number>**  
**`!~: <aData>!~;`** ne pas interpréter les instructions que pourrait contenir aData jusqu'à **`!~;`**.  
**`!~:; <aData> <\n>`** ne pas interpréter les instructions que pourrait contenir aData jusqu'à un saut de ligne **`<\n>`**.  
**Avec \<number>**  
**`!~<number>: <aData>!~<number>;`** ne pas parser pendant un certain nombres de bits, d'octets ou de caractères (les deux \<number> doivent être identique).  
Avec \<number>=**x** c'est un nombre de caractères à sauter.  
Et pour des aData binaire ([en option](#optionBin)) \<number>=**nombre*x** => nombre de bits à sauter si x=1, nombre d'octets à sauter pour x=8 ...  

#### Start a pointMarkup

Première instruction (générée par défaut si absente) d'un pointMarkup.  
Permet aussi d'insérer un pointMarkup dans un autre.  
**`!~*[<baseExtension>][[<+|-><extension>][<+|-><extension>]...<+|-><extension>]`**  
\<baseExtension> par défaut celle du parseur.  

#### Dernière date de modification

**`!~`**

#### Indication pointMarkup langage (extension)  

? **`!~[<+|-><extension>][<+|-><extension>]...<+|-><extension>`**  
~ **`!..[<+|-><extension>][<+|-><extension>]...<+|-><extension>`**  
**`<+> demande la prise en charge d'un extension.`**  
**`<-> retire la prise en charge d'une extension.`**  
**`Les commands communes ne peuvent être exclues.`**  

Par défaut c'est l'extension du pointMarkup si contenu dans un fichier.  

#### <a name="markersChange">Utiliser un \<markers> Unicode </a>

**`!<newMarkers>\n`** uniquement en début et fin d'un pointMarkup.

Permis avec un parser qui supporter l'Unicode mais déconseillé pour risques de confusion notamment lors de copie coller de bout de code pointMarkup d'un pointMarkup à un autre.  
Pour ce faire le pointMarkup à parser doit commencer et ce terminer par une même ligne contenant l'instruction **`!<newMarkers>`** ou **`<newMarkers>`** est le marqueur utiliser codé non ASCII et en Unicode. Choisir ce newMarker en veillant aux conflits d'interprétation.

<br><hr style="height:8px; background-color:rgb(209, 87, 112);border:0px">
<span id="dtEmbInst" style="float: right;">[Retour index](#docIndex)</span>

## <a name="nameSpace">Espace de noms et dataFlux</a>

L'espace de nom est étroitement lié au dataFlux.  
Pour rappel les instructions fonctionnent comme un markup langage. Ceci implique une organisation hiérarchique en arbre du fait de l'imbrication possible d'instructions.  

### <a name="exchangeUser">_Échanges utilisateurs_</a>

- **echo:** Sauf exception ce qui est dans un **pointMarkup** est **transmit** à **echo** ( console, file, screen, data ... ), de haut en bas, lors du décodage.  
- **input** sert à recevoir des données.  
- **output** sert pour informer ou demander une réponse utilisateur.  
- **log** est le journal d'évènements.  

### _dataFlux_

Les instructions disposent de plusieurs flux:

- Entrant
  - aDataFlux ( parser amont )
  - paramFlux ( paramètre pour type fonction )
  - **input** ( recevoir des données ou réponses utilisateur )
- Sortant
  - aDataFlux (parent par défaut) c'est **echo** (parser aval).  
  Ce flux est au format Texte, html ou binaire interne.
  - **output** ( informer ou demander une réponse utilisateur ).  
  - **log** ( journal d'évènements ).  
- En interne
  - **stackInst:** La pile des instructions.  

### _Espace de noms_

Les instructions disposent de plusieurs espaces selon \<target> et/ou \<property>

- variables  
Réunit plusieurs espaces qui peuvent contenir des données ou méthodes.
  - public
  - privée
  - temporaire privée
- étiquette
- ancre (id)
- commentaire
- liens externe

Un peu à part il y a aussi la pile de paramètres.

### Données

? Pour la base les données reste simplement au format content-type 'text' et peuvent contenir des variable remplaçables.

### <a name="methodes">Méthodes</a>

Les méthodes qu'un pointMarkup peut construire sont limitées à un simple system de template. Remplacement poste à poste de variables contenue dans une données par des paramètres (argument) et d'en renvoyer le résultat. Elles retournent toujours du content-type texte.

**_Méthodes hosted_**  

L'absence d'instructions de programmation standard (arithmétique, logique, boucle) dans la syntaxe de base d'un pointMarkup peut surprendre. Mais c'est un choix qui a été fait pour les limites de la syntaxe de base afin de laisser au langages, qui utilisent cette syntaxe, une certaine liberté dans ses besoins.  
Pour combler ce manque il a été mis en place la possibilité de faire appel à des méthodes internes au parser ou externes (sorte de plugin). Celles-ci retourneront toujours du texte mais elles ont généralement accès aux données interne du pointMarkup.  

<br><hr style="height:8px; background-color:rgb(209, 87, 112);border:0px">
<span id="dtEmbInst" style="float: right;">[Retour index](#docIndex)</span>

## Particularités

- Un pointMarkup ne contient pas obligatoirement des instructions. Par conséquent beaucoup de fichier ( de content/type 'text' ) sont compatible pointMarkup.  

### <a name="ttmUnderscore">Traitement underscore</a>

Un underscore est un agrément visuel. Il est généralement ignoré à l'interprétation.  

- Une commande peut donc contenir des underscores à n'importe quel emplacement sauf pour un underscore **unique en début ou en fin** de commande qui peuvent avoir respectivement une signification dans \<target> et \<property>.  
- Une suite de underscore autre que en début ou fin de commande peut dans certaines exceptions précisent être remplacée par un espace au lieu d'être ignoré.  

### <a name="CommentWhite">Commentaire ou caractère blanc avant instruction</a>

? ajouter une [instruction parser](#parserInst) pour indique des marques de commentaire non encore gérés.  

- Si il existe ( en sens contraire de la lecture ) une suite de caractères blanc jusqu'à un saut de ligne cette suite et le saut de ligne sont supprimés. Cette règle est prioritaire.  
- Si il existe un saut de ligne avant une instruction celui-ci sera supprimé.
- **?** Toute marque de commentaire **`/* ou //`** précédent immédiatement une instruction sera **retirée** de aData qui la précède **sauf** pour le flux **echo**.  

### <a name="tabEnterData">Tab (\t) in aData</a>

Par défaut les tabulations sont conservées mais pas forcément effectives. 

### <a name="cmdBlancData">Caractères blancs entre commands et aData</a>

- **!** la séquence tabulation saut de ligne entre commands et aData indique de ne pas conserver les tabulations en début de lignes.  

### <a name="spaceEnter">Saut de ligne précédé d'un espace</a>

La séquence espace saut de ligne est considérée par le parser comme un un espace qui sera effectivement remplacée par celui-ci si nécessaire.

### Parsers BT-RL multi-tree

Un pointMarkup est conçu comme un mur sociale. Ce qui est connu est en bas.  
Le flux du parsers d'un pointMarkup va donc de bas en haut et de droite à gauche l'inverse de l'écriture, ce qui existe en haut (top document, **parser aval**, downstream ) ne peut être immédiatement utilisé d'en bas (bottom document, **parser amont**, upstream ). \<target> peut conduire à une forme d'analyse multi-tree.

Pour les instruction de séquence, le parser fait un deuxième passage de haut en bas du début à la fin de la séquence. 

Parser autonome sortie echo ou avec interface.  

Avec la possibilité de persistence des données et méthodes.  
Parser héberger (hosted) de type library, framework généralement écrit dans le langage du 'host'.  
Parser server.

<br><hr style="height:8px; background-color:rgb(209, 87, 112);border:0px">
<span id="dtEmbInst" style="float: right;">[Retour index](#docIndex)</span>

## Options

### <a id="optionBin" >aData binaire</a>

Pour embarquer des données binaire certain langages compatible pointMarkup peuvent utiliser l'[instruction de pause d'interprétation](pauseInst)  
**`!~<number>: <aData>!~<number>;`**  
avec \<number>=**nombre*x** => nombre de bits à sauter si x=1, nombre d'octets à sauter pour x=8 ...  
Ceci force le parser à ne pas analyser pendant un certain nombres de bits d'octets ...  

### pistes  

- **?** Le Parsers en mode streaming s'inverse et devient standard TB-LR.  
- **?** Le \<markers> par défaut peut être changé.  
  **&** Permettre d'autre \<markers> pour commandsNext commandsEnd.  
  **&** Se défend visuellement mais complique la reconnaissance.  
  **&** Non pour l'instant il reste ouvert de pouvoir utiliser différent marqueur précis selon l'utilité.
- ? tout saut de ligne précédé d'un seul espace sera supprimé ( voir exceptions dans [caractère blanc avant instruction](#CommentWhite) ).  
& Non c'est géré par les languages.  


***

<!--
## Autres
-->
***