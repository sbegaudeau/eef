---
layout: specification
description: EEF Core
---

This section presents the model concepts from platform:/resources/org.eclipse.eef/model/eef.ecore

#### View

#### Page 

#### Group

#### Container

#### Widgets

A widget is represented in the metamodel thanks to the `EEFWidgetDescription` contained in an `EEFContainerDescription` by the `widgets` reference.

* `EEFWidgetDescription` : Description of a graphical element.
* `EEFContainerDescription#widgets [1..*] containment` : References the widgets to hold.
* `EEFWidgetDescription#identifier: EString [1..1]` : Used to identify a specific widget.
* `EEFWidgetDescription#labelExpression: EString return String [0..1]` : Interpreted expression providing the label of the widget visible by the end-users.

##### Label

* `EEFLabelDescription extends EEFWidgetDescription` : Represents a label in the user interface.

##### Text / Text area

* `EEFTextDescription extends EEFWidgetDescription` : Represents a text field in the user interface.
* `EEFTextDescription#valueExpression: EString return String [0..1]` : Interpreted expression indicating how to display the input value.
* `EEFTextDescription#editExpression: EString return String [0..1]` : Interpreted expression defining the behavior executed when the end-user updates the value of the text field. The `newValue` variable is available which represents the updated value.
* `EEFTextDescription#lineCount: EInt [0..1]` : Must be upper or equal to 1. If lineCount > 1, the text field is represented thanks to a text area.

##### Checkbox

* `EEFCheckboxDescription extends EEFWidgetDescription` : Represents a checkbox used to edit a boolean property.
* `EEFCheckboxDescription#valueExpression: EString return String [0..1]` : Interpreted expression indicating how to display the input value.
* `EEFCheckboxDescription#editExpression: EString return String [0..1]` : Interpreted expression defining the behavior executed when the end-user updates the value of the checkbox field. The `newValue` variable is available which represents the updated value.

##### Select

* `EEFSelectDescription extends EEFWidgetDescription` : Represents a collection of candidates used to edit a single or multi-valued property.
* `EEFSelectDescription#valueExpression: EString return String [0..1]` : Interpreted expression providing the initial selected value of the radio.
* `EEFSelectDescription#editExpression: EString return String [0..1]` : Interpreted expression defining the behavior executed when the end-user updates the value of the combo. The `newValue` variable is available which represents the updated value.
* `EEFSelectDescription#candidatesExpression: EString return String [0..1]` : Interpreted expression defining the various proposals available.
* `EEFSelectDescription#candidateDisplayExpression: EString return String [0..1]` : Interpreted expression indicating how to display the input value. The `candidate` variable is available which represents the current candidate that should be displayed.

##### Radio group

* `EEFRadioDescription extends EEFWidgetDescription` : Represents a radio group in the user interface.
* `EEFRadioDescription#valueExpression: EString return String [0..1]` : Interpreted expression providing the initial selected value of the radio.
* `EEFRadioDescription#editExpression: EString return String [0..1]` : Interpreted expression defining the behavior executed when the end-user updates the value of the radio. The `newValue` variable is available which represents the updated value.
* `EEFRadioDescription#candidatesExpression: EString return String [0..1]` : Interpreted expression defining the various proposals available.
* `EEFRadioDescription#candidateDisplayExpression: EString return String [0..1]` : Indicates how to display the input value. The `candidate` variable is available which represents the current candidate that should be displayed.

##### Button

* `EEFButtonDescription extends EEFWidgetDescription` : Represents a button in the user interface.
* `EEFButtonDescription#buttonLabelExpression: EString return String [0..1]` : Label of the button visible in the user interface.
* `EEFButtonDescription#pushExpression: EString return String [0..1]` : Interpreted expression defining the behavior executed when the end-user pushed the button.

