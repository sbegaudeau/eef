---
layout: specification
description: Sirius properties
---

The purpose of this section is to describe the Properties metamodel from platform:/resource/org.eclipse.sirius.properties/properties.ecore.

#### Properties Metamodel

##### Widget

A widget is represented in the metamodel thanks to the `WidgetDescription` contained in an `ContainerDescription` by the `widgets` reference.

* `WidgetDescription` : Description of a graphical element.
* `ContainerDescription#widgets [1..*] containment` : References the widgets to hold.
* `WidgetDescription#identifier: EString [1..1]` : Used to identify a specific widget.
* `WidgetDescription#labelExpression: InterpretedExpression return String [0..1]` : Interpreted expression providing the label of the widget visible by the end-users.

###### Label

* `LabelDescription extends WidgetDescription` : Represents a label in the user interface.

###### Text

* `TextDescription extends WidgetDescription` : Represents a text field in the user interface.
* `TextDescription#valueExpression: InterpretedExpression return String [0..1]` : Interpreted expression indicating how to display the input value.
* `TextDescription#initialOperation: InitialOperation [1..1]` : Define the behavior executed when the end-user updates the value of the text field. The `newValue` variable is available which represents the updated value.

###### Text Area

* `TextAreaDescription extends TextDescription` : Represents a text area in the user interface.
* `TextAreaDescription#lineCount: EInt [0..1]` : Must be upper or equal to 1. If lineCount > 1, the text field is represented thanks to a text area.

###### Checkbox

* `CheckboxDescription extends WidgetDescription` : Represents a checkbox used to edit a boolean property.
* `CheckboxDescription#valueExpression: InterpretedExpression return String [0..1]` : Interpreted expression indicating how to display the input value.
* `CheckboxDescription#initialOperation: InitialOperation [1..1]` : Defines the behavior executed when the end-user updates the value of the checkbox field. The `newValue` variable is available which represents the updated value.

###### Radio group

* `RadioDescription extends WidgetDescription` : Represents a radio group in the user interface.
* `RadioDescription#valueExpression: InterpretedExpression return String [0..1]` : Interpreted expression providing the initial selected value of the radio.
* `RadioDescription#initialOperation: InitialOperation [1..1]` : Defines the behavior executed when the end-user updates the value of the radio. The `newValue` variable is available which represents the updated value.
* `RadioDescription#candidatesExpression: InterpretedExpression return String [0..1]` : Interpreted expression defining the various proposals available.
* `RadioDescription#candidateDisplayExpression: InterpretedExpression return String [0..1]` : Indicates how to display the input value. The `candidate` variable is available which represents the current candidate that should be displayed.

###### Select

* `SelectDescription extends WidgetDescription` : Represents a collection of candidates used to edit a single or multi-valued property.
* `SelectDescription#valueExpression: InterpretedExpression return String [0..1]` : Interpreted expression providing the initial selected value of the radio.
* `SelectDescription#initialOperation: InitialOperation [1..1]` : Defines the behavior executed when the end-user updates the value of the combo. The `newValue` variable is available which represents the updated value.
* `SelectDescription#candidatesExpression: InterpretedExpression return String [0..1]` : Interpreted expression defining the various proposals available.
* `SelectDescription#candidateDisplayExpression: InterpretedExpression return String [0..1]` : Interpreted expression indicating how to display the input value. The `candidate` variable is available which represents the current candidate that should be displayed.

###### Button

* `ButtonDescription extends WidgetDescription` : Represents a button in the user interface.
* `ButtonDescription#buttonLabelExpression: InterpretedExpression return String [0..1]` : Label of the button visible in the user interface.
* `ButtonDescription#pushExpression: initialOperation: InitialOperation [1..1]` : Defines the behavior executed when the end-user pushed the button.