---
layout: specification
description: Roadmap
---


##### M4 11/12/15 +4

demo industrialisée avec :

- text, checkbox, combo
- context (viewpoint, representation, layer, filter, mapping, model explorer)
  - model explorer ~> absence de contexte avec le bridge Sirius ?
- Mode par défaut du DSL, surcharge de partie du comportement par défaut 
  - Définition du comportement attendu
  - Définition des règles par défaut (mise à jour du properties.ecore?)
  - Définition du comportement attendu pour la surcharge (remplacement, position, ordre, génération dans le odesign ou pas)
  - Définition d'exemples de surcharge
  - Conservation ou suppression des autres onglets (GMF, Advanced, Semantic)
  - Ordre des onglets
- Doc technique
- Tests + job
- Releng : produire un update site consommable par sirius
- API/étendre le DSL pour décrire des widget custom + converteur (y réfléchir)
  - Définition dans le DSL (Comment ? Migration avec évolution de properties.ecore ?)
  - API Java
  - Exemple avec color picker

Cycle de vie clair et maitrisé :

- prendre en compte la validation
- read only
- Se renseigner sur les possibilités de SWT d'un point de vue montée en charge / perf / dynamicité

expression.ecore & API interpreter dans sirius

###### Widgets for M4

- Text
- Checkbox
- Combo


##### M5 29/01/16 +7

- Dialog & wizard (dérisqué)
- emf.edit, (clarifier les besoins, pour lancer le dev dans sirius pour se brancher dessus dans M6)
  - Utilisation de label provider
  - Choice of value
  - Diagnostician (message, feature, context, etc)
  - EDataType validation (see [this](http://eclipsesource.com/blogs/2014/08/26/emf-validation-for-datatype-constraints/))
  - eSet / eGet
- I18N des VSM côté Sirius exploitable par eef (EMF.edit par défaut)
- Implémentation du widget custom
- 1 widget complexe (link eref viewer)
  - Trouver un nouveau nom :)
  - Définition dans properties.ecore
- Undo partiel (opérations multi étapes/cancel) (best practices)
- Multi selection définition du besoin, 1er proto pour mise en place de la structure pour implem dans M6)
- Doc technique
- Tests


###### Widgets for M5

- Label
- Hyperlink
- Radio
- TextArea
- Button (test avec dialog -> browse button with fileselector /!\ cycle de vie)


##### M6 26/03/16 (API freeze) +8

- Dialog et wizard implem
- Validation
  - Message and kind on widgets, groups, view ([message manager](https://www.eclipse.org/eclipse/platform-ua/proposals/forms/enhancements-3.3/))
  - Markers and Problems view
- Styling
  - Font
  - Colors (background, foreground)
  - Conditional style
  - Label (bold, italic, underline, strikethrough etc)
- widgets ++
- Doc technique
- Tests

###### Widgets for M6

- Tree
- Table
  - actions: appearance and behavior (add, remove, up, down)


##### M7 06/05/16 +6

- Raffiner le DSL de description(Reuse, import, autre tooling), 
- performances/montée en charge, 
- layout
- widgets ++
- Doc technique
- Tests
- Documentation utilisateur


##### RC1 20/05/16

##### Eclipse Neon 1

- Quickfixes
- content assist

##### Eclipse Neon 2


##### Eclipse Omega

- tooltip
- actions on the view
  - show advanced
  - restore default value
  - see related elements
