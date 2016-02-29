---
layout: specification
description: Widgets
---

This section presents the widget concept.

#### Default widgets

The EEF properties tab framework supports the following widgets by default.
For details about widgets metamodel see the ![EEF metamodel section]({{ site.baseurl }}specifications/architecture/eef/metamodel.html)

##### Label

The label widget is used to represent a non editable text description:

![Label]({{ site.baseurl }}specifications/architecture/eef/images/Label.png)

##### Text / TextArea

The text widget is used to represent an editable text:

![Text]({{ site.baseurl }}specifications/architecture/eef/images/Text.png)

The text area is used to represent an editable multiline text:

![TextArea]({{ site.baseurl }}specifications/architecture/eef/images/TextArea.png)

##### Checkbox

The checkbox widget is used to represent a boolean :

![Checkbox]({{ site.baseurl }}specifications/architecture/eef/images/Checkbox.png)

##### Radio Group

The radio group widget is used to represent a unique choice :

![Radio Group]({{ site.baseurl }}specifications/architecture/eef/images/Radio.png)

##### Button

The button widget is used to represent a click action :

![Button]({{ site.baseurl }}specifications/architecture/eef/images/Button.png)

##### Select

The select widget is used to represent a combo box :

![Select]({{ site.baseurl }}specifications/architecture/eef/images/Select.png)

##### Single reference

The single reference widget is used to represent a mono valued reference:

![Single Reference]({{ site.baseurl }}specifications/architecture/eef/images/SingleReference.png)

##### Multiple references

The multiple references widget is used to represent a multi valued reference :

![Multiple References]({{ site.baseurl }}specifications/architecture/eef/images/Multiple References.png)

#### Implementation

All the widgets are implemented using the same pattern.

##### LifecycleManager

A widget implements the ILifecycleManager interface. This class is in charge of handling the widget lifecycle :

* `aboutToBeShown` : This operation is called before the appearance of the user interface in order to let us connect the listeners to the widgets. For example, in the case of the _Text_ widget a `ModifyListener` is registered to call the controller to update the value when the user update the text field. This operation is also used to declare the controllers associated to a widget.
* `createControl` : This operation creates the controls for the widget description.
* `refresh` : This operation refreshes the widgets and mainly call the controller refesh.
* `aboutToBeHidden` : This operation prepares for the widgets to be hidden, unregister all the listeners and the controllers' consumers.
* `dispose` : This operation disposes the content created.

##### Controller

The controller is responsible of supporting all the interactions with the widgets created for a widget description. It extends the `IEEFWidgetController` interface.

As described before, the controller is declared during the aboutToBeShown operation.
It is called by the listeners to update the semantic elements associated to a widget and in return to update the widget value in the user interface.

If we have a look to the Text widget, it provides its own controller `EEFTextController` with its associated interface `IEEFTextController`.

An `updateValue` method is declared. This method is called by the `ModifyListener` when the value of the text field is updated by the user.

#####Consumer

The controller calls also the different consumers registered for a widget. These consumers are declared in the `aboutToBeShown` method. They provide an `apply` method which will be called by the consumer.

For example on the Text widget, two consumers are declared : `onNewLabel` and `onNewValue`. These consumers update, in the UI, the text widget field and label when the value or the label is updated.

#### Default behavior

#### Custom widgets