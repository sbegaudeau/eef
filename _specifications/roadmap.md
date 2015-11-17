---
layout: specification
description: Roadmap
---


##### M4 11/12/15 +4

The goal of this milestons is to build an industrialized demonstration.

###### DSLs **2d**

- remove unnecessary concepts from eef.ecore (domain class?) **2d**

###### Widgets **22d**

- semantic candidate (pages / groups / widgets) **1d**
- text
  - lifecycle (edit initial operation / valueExpression) **1d**
  - refactor the controller and instantiation of the widgets **2d**
- checkbox **2d**
- combo **3d**
- dynamic mappings **3d**
  - for iterator in collectionExpression (domainClass.eAllStructuralFeatures) -> switch switchExpression -> case valueExpression
- context **10d**
  - viewpoint, representation, layer, filter, mapping
  - model explorer ~> lack of context with the Eclipse Sirius bridge

###### Interpreters **17d**

This work will be realized by the Sirius team.

- new interpreter API in sirius (specification and integration) **2d**
  - variables lifecycle (remove setVariables / unsetVariables)
  - completion proposals (EMF 2.8's styled strings for the display and information)
  - camel case support and filters for the interpreters and proposals
- usage of expression.ecore **15d**

###### Default behavior and customization **24d**

- default behavior of properties.ecore and overriding of the default behavior
  - definition of the default behavior expected (each structural should have a default widget in the order of the structural features)
  - definition of the default rules in a default.properties **1d**
  - specification of the behavior expected to override the default behavior **2d**
    - replacement
    - position
    - order
    - should we generate anything in the odesign of the specifier?
    - create some examples
  - implementation of the overriding **10d**
  - how can we keep / remove other tabs provided by other tools (GMF, Advanced, Semantic) **2d**
  - tab order **2d**
- API to extend properties.ecore with custom widgets (convertion in eef.ecore)
  - how to define a custom widget in properties.ecore **2d**
  - how to make this definition evolve with the future changes in properties.ecore (regenerate the code, etc) **1d**
  - java API **2d**
  - very basic prototype with a color picker **2d**

###### Lifecycle **7d**

We need to have a clear vision of the way the lifecycle of our application works

- lifecycle to create the Properties view **1d**
- lifecycle of the interaction with Eclipse Sirius **2d**
- take into account the validation **1d**
- read only **1d**
- evaluate the performances and capabilities of SWT **2d**
  - ability to create and destroy widgets (hide them?)
  - scaling (how many widgets? are pages not displayed refreshed?)

###### Release engineering and project management **4d**

- technical documentation **2d**
- unit tests, continuous integration and integration with Gerrit **2d**
- contribute to the EEF update site for Eclipse Sirius

###### Widgets for M4

- text
- checkbox
- combo

**Expected cost: 76d**

##### M5 29/01/16 +7

###### Dialogs and wizards **10d**

We need to evaluate how we can create dialogs & wizards

- undo / cancel (best practices)
- UI thread / jobs interactions

###### EMF Edit **7d**

We need to specify how to integrate emf.edit. This work will be realized by the Sirius team.

- label provider
- choice of value
- diagnostician (message, feature, context, etc)
- EDataType validation (see [this](http://eclipsesource.com/blogs/2014/08/26/emf-validation-for-datatype-constraints/))
- eSet / eGet
- evaluation of the usage I18N form Sirius in eef (fallback to EMF.edit by default)
- update of the default behavior to use EMF Edit

###### Widgets

- final implementation of the custom widget (color picker) **1d**
- creation of a complex widget (link eref viewer) **10d**
  - find its new name :)
  - definition in properties.ecore
  - implementation
- support for multi selection **15d**
  - evaluate the need for this feature and its behavior (what should we display)
  - 1st prototype and final implementation in M6

###### Release engineering and project management **4d**

- technical documentation
- more tests

###### Widgets for M5

- label **2d**
- hyperlink **3d**
- radio **2d**
- textArea **1d**
- button (test with dialogs -> browse button with fileselector /!\ lifecycle) **3d**

**Expected cost: 58d**


##### M6 26/03/16 (API freeze) +8

###### Dialogs and wizards **20d**

Implementation of the dialogs et wizards

###### Validation and messages **15d**

- Message and kind on widgets, groups, view ([message manager](https://www.eclipse.org/eclipse/platform-ua/proposals/forms/enhancements-3.3/))
- Markers and Problems view

###### Styling **15d**

Support for styling **10d**

- Font
- Colors (background, foreground)
- Label (bold, italic, underline, strikethrough etc)
- Conditional style **5d**

###### Release engineering and project management **5d**

- technical documentation
- more tests

###### Widgets for M6 **35d**

- Tree **15d**
- Table **20d**
  - actions: appearance and behavior (add, remove, up, down)


**Expected cost: 80d**

##### M7 06/05/16 +6

###### Tooling **15d**

- advanced features in properties.ecore
  - reuse / import
  - new tooling

###### Performances **2d**

- performance and scaling (fix everything!)

###### Layout **10d**

- grid layout with containers?

###### Release engineering and project management **5d**

- technical documentation
- user documentation
- more tests

**Expected cost: 32d**

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
