---
layout: specification
description: Roadmap
---


##### M4 11/12/15 +4

The goal of this milestons is to build an industrialized demonstration.

###### Mappings

- text
  - lifecycle (edit initial operation / valueExpression)
- checkbox
- combo
- semantic candidate (pages / groups / widgets)
- dynamic mappings
  - for iterator in collectionExpression (domainClass.eAllStructuralFeatures) -> switch switchExpression -> case valueExpression
- context 
  - viewpoint, representation, layer, filter, mapping
  - model explorer ~> lack of context with the Eclipse Sirius bridge

###### Interpreters

- new interpreter API in sirius
  - variables lifecycle (remove setVariables / unsetVariables)
  - completion proposals (EMF 2.8's styled strings for the display and information)
  - camel case support and filters for the interpreters and proposals
- usage of expression.ecore

###### Default behavior and customization

- default behavior of properties.ecore (if the user does not specify anything) and overriding of the default behavior
  - definition of the default behavior expected (each structural should have a default widget in the order of the structural features)
  - definition of the default rules in a default.properties
  - definition of the behavior expected to change the default behavior
    - replacement
    - position
    - order
    - should we generate anything in the odesign of the specifier?
  - create some examples
  - how can we keep / remove other tabs provided by other tools (GMF, Advanced, Semantic)
  - tab order
- API to extend properties.ecore with custom widgets (convertion in eef.ecore)
  - how to define a custom widget in properties.ecore
  - how to make this definition evolve with the future changes in properties.ecore (regenerate the code, etc)
  - Java API
  - Example with a color picker

###### Lifecycle

We need to have a clear vision of the way the lifecycle of our application works

- lifecycle to create the Properties view
- lifecycle of the interaction with Eclipse Sirius
- take into account the validation
- read only
- evaluate the performances and capabilities of SWT
  - ability to create and destroy widgets (hide them?)
  - scaling (how many widgets? are pages not displayed refreshed?)

###### Release engineering and project management

- Technical documentation
- Unit tests, continuous integration and integration with Gerrit
- Contribute to the EEF update site for Eclipse Sirius

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
- lock
