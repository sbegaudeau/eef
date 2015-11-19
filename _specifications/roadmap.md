---
layout: specification
description: Roadmap
---


##### M4 11/12/15 availability 4 weeks or 30 days (planning 24 days)

The goal of this milestone is to build an industrialized prototype.
At the end of this milestone, we provide an update site for EEF and an update site for a specific Sirius version. 
With this milestone a Sirius specifier is able to specify in a odesign file a new properties view description. He is able to create in the VSM properties views, tabs, sections and Text widgets. Following the Sirius philosophy, the properties view description is interpreted at runtime by Sirius and is based on binding description. Same as in classical Sirius representation, the mapping are based on semantic candidates defined with domain class and interpreted expressions.
When the end user selects a diagram element in a Sirius based modeler, the properties view shows the selected element properties. Only text widget elements are available.

###### DSLs

- remove unnecessary concepts from eef.ecore (domain class?) **2d**

###### Widgets

- semantic candidate (pages / groups / widgets) **1d**
- text
  - lifecycle (edit initial operation / valueExpression) **1d**
  - refactor the controller and instantiation of the widgets **2d**

###### Interpreters

This work will be realized by the Sirius team.

- new interpreter API in sirius (specification and integration) **2d**
  - variables lifecycle (remove setVariables / unsetVariables)
  - completion proposals (EMF 2.8's styled strings for the display and information)
  - camel case support and filters for the interpreters and proposals

###### Default behavior and customization

- default behavior of properties.ecore
  - definition of the default behavior expected (each structural should have a default widget in the order of the structural features)
  - definition of the default rules in a default.properties **1d**
- how can we keep / remove other tabs provided by other tools (GMF, Advanced, Semantic) **2d**

###### Lifecycle

We need to have a clear vision of the way the lifecycle of our application works

- lifecycle to create the Properties view **1d**
- lifecycle of the interaction with Eclipse Sirius **2d**
- take into account the validation **1d**
- read only **1d**
- evaluate the performances and capabilities of SWT **2d**
  - ability to create and destroy widgets (hide them?)
  - scaling (how many widgets? are pages not displayed refreshed?)

###### EMF Edit

Specification with the Sirius team **2d**

###### Release engineering and project management

- technical documentation **2d**
- unit tests, continuous integration and integration with Gerrit **2d**
- contribute to the EEF update site for Eclipse Sirius


##### M5 29/01/16 availability 7 weeks or 50 days (planning 45 days)

The goal of this milestone is to support basic widgets and dynamic mappings.
At the end of this milestone, we provide some more basic widgets : Checkbox, Combo, Label, Radio, TextArea, Button.
It is also possible for the specifier to define dynamic mappings to specify with one mapping description many widgets. For example, it will be possible to define that 'EString' attributes defined in the meta-model will be represented with a Text widget.
At the end of this milestone, when the specifier does not specify anything for properties view in the VSM, at runtime a default properties view will be provided.
The specifier can also define that a properties view is visible only when a Sirius element exists : Viewpoint, Representation, Layer, Filter, Mapping. For example, this means that when the end user selects an element in a diagram if the 'Documentation' layer is activated, he will see a new documentation tab with a text area. If this layer is unactivated, this tab will not be visible anymore.

###### Widgets

- checkbox **2d**
- combo **3d**
- label **2d**
- radio **2d**
- textArea **1d**
- button (test with dialogs -> browse button with fileselector /!\ lifecycle) **3d**

- dynamic mappings **3d**
  - for iterator in collectionExpression (domainClass.eAllStructuralFeatures) -> switch switchExpression -> case valueExpression
- final implementation of the custom widget (color picker) **1d**

- context **10d**
  - viewpoint, representation, layer, filter, mapping
  - model explorer ~> lack of context with the Eclipse Sirius bridge

- tab order **2d**

###### EMF Edit

Feedback and improvement with the Sirius team **2d**

###### Specification of the styling **10d**

- Font
- Colors (background, foreground)
- Label (bold, italic, underline, strikethrough etc)
- Conditional style

###### Release engineering and project management **4d**

- technical documentation
- more tests


##### M6 26/03/16 (API freeze) availability 8 weeks or 65 days (planning 57 days)

The goal of this milestone is to support EMF edit by default and get a first version of validation and styling.
By default, EEF uses EMF Edit : label provider, choice of values, diagnostician, validation.
The specifier can provide custom complex widgets to his properties view description thanks to a dedicated API.
For the end user, when a validation error is thrown, the associated element in the properties view is highlighted (marker, color change).
It is possible for the specifier to provide styles for widgets. For example for a Text widget, he will be able to change the font and the foreground/background color. As in Sirius, he can also specify conditional style on widget description based on a predicate.

###### Feedback

Fix and improvements **10d**

###### Widget

- API to extend properties.ecore with custom widgets (conversion in eef.ecore)
  - how to define a custom widget in properties.ecore **2d**
  - how to make this definition evolve with the future changes in properties.ecore (regenerate the code, etc) **1d**
  - java API **2d**
  - very basic prototype with a color picker **2d**

###### EMF Edit

Integration of the EMF Edit support created by the Sirius team **5d**

- label provider
- choice of value
- diagnostician (message, feature, context, etc)
- EDataType validation (see [this](http://eclipsesource.com/blogs/2014/08/26/emf-validation-for-datatype-constraints/))
- eSet / eGet
- evaluation of the usage I18N form Sirius in eef (fallback to EMF.edit by default)
- update of the default behavior to use EMF Edit

###### Validation and messages

Development **10d**

- Message and kind on widgets, groups, view ([message manager](https://www.eclipse.org/eclipse/platform-ua/proposals/forms/enhancements-3.3/))
- Markers and Problems view

###### Styling

Implementation of the styling **20d**

###### Release engineering and project management **5d**

- technical documentation
- more tests

##### M7 06/05/16 availability 6 weeks or 50 days (planning 52 days)

The goal of this milestone is to fix performance/scale issue. We provide also layout capabilities and some basic tooling to help the specifier to define his properties view description.

###### Feedback

Fix and improvements **20d**

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


##### RC1 20/05/16

##### Eclipse Neon 1

- quickfixes
- content assist

###### Interpreters

- usage of expression.ecore **15d**

###### Behavior
- specification of the behavior expected to override the default behavior **2d**
  - replacement
  - position
  - order
  - should we generate anything in the odesign of the specifier?
  - create some example
  - implementation of the overriding **10d**

##### Eclipse Neon 2

- sleep

###### Dialogs and wizards **10d**

We need to evaluate how we can create dialogs & wizards

- undo / cancel (best practices)
- UI thread / jobs interactions


###### Dialogs and wizards **20d**

Implementation of the dialogs et wizards

##### Eclipse Omega

- Tree **15d**
- Table **20d**
  - actions: appearance and behavior (add, remove, up, down)

- tooltip
- actions on the view
  - show advanced
  - restore default value
  - see related elements

- lock

- support for multi selection **15d**
  - evaluate the need for this feature and its behavior (what should we display)
  - 1st prototype and final implementation in M6

- creation of a complex widget (link eref viewer) **10d**
  - find its new name :)
  - definition in properties.ecore
  - implementation


