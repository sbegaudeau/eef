---
layout: specification
description: Roadmap
---


##### M4 11/12/15 +4

The goal of this milestons is to build an industrialized demonstration.

###### Widgets

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

This work will be realized by the Sirius team.

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

- technical documentation
- unit tests, continuous integration and integration with Gerrit
- contribute to the EEF update site for Eclipse Sirius

###### Widgets for M4

- text
- checkbox
- combo


##### M5 29/01/16 +7

###### Dialogs and wizards

We need to evaluate how we can create dialogs & wizards

- undo / cancel (best practices)
- UI thread / jobs interactions

###### EMF Edit

We need to specify how to integrate emf.edit. This work will be realized by the Sirius team.

- label provider
- choice of value
- diagnostician (message, feature, context, etc)
- EDataType validation (see [this](http://eclipsesource.com/blogs/2014/08/26/emf-validation-for-datatype-constraints/))
- eSet / eGet
- evaluation of the usage I18N form Sirius in eef (fallback to EMF.edit by default)

###### Widgets

- implementation of the custom widget
- creation of a complex widget (link eref viewer)
  - find its new name :)
  - definition in properties.ecore
  - implementation
- support for multi selection
  - evaluate the need for this feature and its behavior (what should we display)
  - 1st prototype and final implementation in M6

###### Release engineering and project management

- technical documentation
- more tests


###### Widgets for M5

- label
- hyperlink
- radio
- textArea
- button (test with dialogs -> browse button with fileselector /!\ lifecycle)


##### M6 26/03/16 (API freeze) +8

###### Dialogs and wizards

Implementation of the dialogs et wizards

###### Validation

- Message and kind on widgets, groups, view ([message manager](https://www.eclipse.org/eclipse/platform-ua/proposals/forms/enhancements-3.3/))
- Markers and Problems view

###### Styling

- Font
- Colors (background, foreground)
- Conditional style
- Label (bold, italic, underline, strikethrough etc)

###### Release engineering and project management

- technical documentation
- more tests

###### Widgets for M6

- Tree
- Table
  - actions: appearance and behavior (add, remove, up, down)


##### M7 06/05/16 +6

###### Tooling

- advanced features in properties.ecore
  - reuse / import
  - new tooling

###### Performances

- performance and scaling (fix everything!)

###### Layout

- grid layout with containers?

###### Release engineering and project management

- technical documentation
- user documentation
- more tests

##### RC1 20/05/16

##### Eclipse Neon 1

- quickfixes
- content assist

##### Eclipse Neon 2

- sleep

##### Eclipse Omega

- tooltip
- actions on the view
  - show advanced
  - restore default value
  - see related elements
- lock
