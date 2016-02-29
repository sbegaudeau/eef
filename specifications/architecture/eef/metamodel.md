---
layout: specification
description: EEF Core
---

This section presents the model concepts from platform:/resources/org.eclipse.eef/model/eef.ecore

#### View

A view is the root concept of the EEF metamodel. A view defines pages, groups of widgets, semantic mappings and related metamodels.

* `EEFViewDescription` : Description of a dynamic contribution to the properties view.
* `EEFViewDescription#identifier: EString [1..1]` : Used to identify a specific view.
* `EEFViewDescription#labelExpression: EString return String [0..1]` : Interpreted expression providing the label of the view visible by the end-users.
* `EEFViewDescription#groups [0..*] containment` : References the groups to hold.
* `EEFViewDescription#pages [1..*] containment` : References the pages to hold.
* `EEFViewDescription#ePackages [0..*] containment` : References the ePackages to hold.

#### Page

A page is a tab represented in the properties view.

![Page]({{ site.baseurl }}specifications/architecture/eef/images/PageMetamodel.png)

* `EEFPageDescription` : Description of a tab.
* `EEFPageDescription#identifier: EString [1..1]` : Used to identify a specific page.
* `EEFPageDescription#labelExpression: EString return String [0..1]` : Interpreted expression providing the label of the page visible by the end-users.
* `EEFPageDescription#semanticCandidateExpression: EString return String [0..1]` : Interpreted expression providing the element which is represented.
* `EEFPageDescription#groups: EString return String [0..1]` : References the groups of widgets which are visible in the page.

#### Group

A group is a section in the properties view.

![Group]({{ site.baseurl }}specifications/architecture/eef/images/GroupMetamodel.png)

* `EEFGroupDescription` : Description of a section.
* `EEFGroupDescription#identifier: EString [1..1]` : Used to identify a specific group.
* `EEFGroupDescription#labelExpression: EString return String [0..1]` : Interpreted expression providing the label of the page visible by the end-users.
* `EEFGroupDescription#semanticCandidateExpression: EString return String [0..1]` : Interpreted expression providing the element which is represented.
* `EEFPageDescription#groups: EString return String [0..1]` : References the sub groups of widgets which are visible in the group.

#### Container

![Container]({{ site.baseurl }}specifications/architecture/eef/images/ContainerMetamodel.png)

#### Widgets

A widget is represented in the metamodel thanks to the `EEFWidgetDescription` contained in an `EEFContainerDescription` by the `widgets` reference.

* `EEFWidgetDescription` : Description of a graphical element.
* `EEFContainerDescription#widgets [1..*] containment` : References the widgets to hold.
* `EEFWidgetDescription#identifier: EString [1..1]` : Used to identify a specific widget.
* `EEFWidgetDescription#labelExpression: EString [0..1] return String [0..1]` : Interpreted expression providing the label of the widget visible by the end-users.

![Widget]({{ site.baseurl }}specifications/architecture/eef/images/WidgetMetamodel.png)


##### Label

![Label]({{ site.baseurl }}specifications/architecture/eef/images/LabelMetamodel.png)

* `EEFLabelDescription extends EEFWidgetDescription` : Represents a label in the user interface.
* `EEFTextDescription#bodyExpression: EString [0..1] return String [0..1]` : Interpreted expression indicating the body of the label containing the meaningful content.

##### Text / Text area

![Text]({{ site.baseurl }}specifications/architecture/eef/images/TextMetamodel.png)

* `EEFTextDescription extends EEFWidgetDescription` : Represents a text field in the user interface.
* `EEFTextDescription#valueExpression: EString [0..1] return String [0..1]` : Interpreted expression indicating how to display the input value.
* `EEFTextDescription#editExpression: EString [0..1] return Void [0..1]` : Interpreted expression defining the behavior executed when the end-user updates the value of the text field. The `newValue` variable is available which represents the updated value.
* `EEFTextDescription#lineCount: EInt [0..1]` : Must be upper or equal to 1. If lineCount > 1, the text field is represented thanks to a text area.

##### Checkbox

* `EEFCheckboxDescription extends EEFWidgetDescription` : Represents a checkbox used to edit a boolean property.
* `EEFCheckboxDescription#valueExpression: EString [0..1] return String [0..1]` : Interpreted expression indicating how to display the input value.
* `EEFCheckboxDescription#editExpression: EString [0..1] return Void [0..1]` : Interpreted expression defining the behavior executed when the end-user updates the value of the checkbox field. The `newValue` variable is available which represents the updated value.

##### Select

* `EEFSelectDescription extends EEFWidgetDescription` : Represents a collection of candidates used to edit a single or multi-valued property.
* `EEFSelectDescription#valueExpression: EString [0..1] return String [0..1]` : Interpreted expression providing the initial selected value of the radio.
* `EEFSelectDescription#editExpression: EString [0..1] return Void [0..1]` : Interpreted expression defining the behavior executed when the end-user updates the value of the combo. The `newValue` variable is available which represents the updated value.
* `EEFSelectDescription#candidatesExpression: EString [0..1] return ecore.EObject [0..-1]` : Interpreted expression defining the various proposals available.
* `EEFSelectDescription#candidateDisplayExpression: EString [0..1] return String [0..1]` : Interpreted expression indicating how to display the input value. The `candidate` variable is available which represents the current candidate that should be displayed.

##### Radio group

* `EEFRadioDescription extends EEFWidgetDescription` : Represents a radio group in the user interface.
* `EEFRadioDescription#valueExpression: EString [0..1] return String [0..1]` : Interpreted expression providing the initial selected value of the radio.
* `EEFRadioDescription#editExpression: EString [0..1] return Void [0..1]` : Interpreted expression defining the behavior executed when the end-user updates the value of the radio. The `newValue` variable is available which represents the updated value.
* `EEFRadioDescription#candidatesExpression: EString [0..1] return ecore.EObject [0..-1]` : Interpreted expression defining the various proposals available.
* `EEFRadioDescription#candidateDisplayExpression: EString [0..1] return String [0..1]` : Indicates how to display the input value. The `candidate` variable is available which represents the current candidate that should be displayed.

##### Button

* `EEFButtonDescription extends EEFWidgetDescription` : Represents a button in the user interface.
* `EEFButtonDescription#buttonLabelExpression: EString [0..1] return String [0..1]` : Label of the button visible in the user interface.
* `EEFButtonDescription#pushExpression: EString [0..1] return Void [0..1]` : Interpreted expression defining the behavior executed when the end-user pushed the button.

##### Single reference

![Single Reference]({{ site.baseurl }}specifications/architecture/eef/images/SingleReferenceMetamodel.png)

* `EEFSingleReferenceDescription extends EEFWidgetDescription` : Represents a button in the user interface.
* `EEFSingleReferenceDescription#valueExpression: EString [0..1] return ecore.EObject [0..1]` : Interpreted expression defining the initial value.
* `EEFMultipleReferencesDescription#displayExpression: EString [0..1] return String [0..1]` : Interpreted expression indicating how to display the input value. The `self` variable is available which represents the current value that should be displayed.
* `EEFSingleReferenceDescription#onClickExpression: EString [0..1] return Void [0..1]` : Interpreted expression defining the behavior executed when the end-user double-clicks on elements in the list. The `selection` variable is available which represents the current selection.
* `EEFSingleReferenceDescription#createExpression: EString [0..1] return Void [0..1]` : Interpreted expression defining the behavior executed when the end-user pushed the create button.
* `EEFSingleReferenceDescription#searchExpression: EString [0..1] return Void [0..1]` : Interpreted expression defining the behavior executed when the end-user pushed the search button.
* `EEFSingleReferenceDescription#unsetExpression: EString [0..1] return Void [0..1]` : Interpreted expression defining the behavior executed when the end-user pushed the unset button. The `selection` variable is available which represents the current selection.

##### Multiple references

![Multiple References]({{ site.baseurl }}specifications/architecture/eef/images/MultipleReferencesMetamodel.png)

* `EEFMultipleReferencesDescription extends EEFWidgetDescription` : Represents a multi valued reference in the user interface.
* `EEFMultipleReferencesDescription#valueExpression: EString [0..1] return ecore.EObject [0..-1]` : Interpreted expression defining the initial values.
* `EEFMultipleReferencesDescription#displayExpression: EString [0..1] return String [0..1]` : Interpreted expression indicating how to display the input value. The `self` variable is available which represents the current value that should be displayed.
* `EEFMultipleReferencesDescription#onClickExpression: EString [0..1] return Void [0..1]` : Interpreted expression defining the behavior executed when the end-user double-clicks on elements in the list. The `selection` variable is available which represents the current selection.
* `EEFMultipleReferencesDescription#createExpression: EString [0..1] return Void [0..1]` : Interpreted expression defining the behavior executed when the end-user pushed the create button.
* `EEFMultipleReferencesDescription#searchExpression: EString [0..1] return Void [0..1]` : Interpreted expression defining the behavior executed when the end-user pushed the search button.
* `EEFMultipleReferencesDescription#unsetExpression: EString [0..1] return Void [0..1]` : Interpreted expression defining the behavior executed when the end-user pushed the unset button. The `selection` variable is available which represents the current selection in the table.
* `EEFMultipleReferencesDescription#upExpression: EString [0..1] return Void [0..1]` : Interpreted expression defining the behavior executed when the end-user pushed the up button. The `selection` variable is available which represents the current selection in the table.
* `EEFMultipleReferencesDescription#downExpression: EString [0..1] return Void [0..1]` : Interpreted expression defining the behavior executed when the end-user pushed the down button. The `selection` variable is available which represents the current selection in the table.
