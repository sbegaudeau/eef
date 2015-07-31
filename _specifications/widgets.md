---
layout: specification
description: Widgets DSL
---

We need to be able to define the widgets which will be used to create the user interface manipulated by the end users.

##### User Stories

###### Zoe - Sirius Specifier

As a Sirius specifier, Zoe wants to have a set of widgets that can be used in order to define how her models will be edited. Zoe, being used to Sirius will want to find concept similar to Sirius ones. She will want to have at least the following kind of widgets:

* text
* checkbox
* select (single and multiple)
* radio
* table

##### Functional Specification

The widgets are contained inside the mapping of the views directly in a similar fashion as Sirius.

###### Text

The concept of Text widget has two attributes. The first one, valueExpression, is an expression used to determine the value to be displayed in the user interface. This value can use the various interpreters available to be computed but it must return a String. For example:

* feature:name
* aql:self.firstName.concat(' ').concat(self.familyName)

The second attribute, editExpression is used to edit the semantic object. This expression would use the edit variable. The edit variable should be created automatically when the user creates a new Text widget. It should have by default the name arg0, this variable will always be a String. As such, the user could easily edit the semantic object using one expression like:

* aql:self.customAQLService(arg0)
* aql:self.eSet(self.eClass().getEStructuralFeature('firstName'), arg0)

While this approach offer some flexibility, some default service may be required in order to streamline the default use case. If an user want to have a simple mapping between a Text widget and an EString EAttribute, the value expression "feature:name" is very easy to setup but the edit expression is a bit more complicated. On the other side it would be very familiar for a Sirius specifier.

###### Checkbox

The concept of Checkbox is very similar to the Text one. The only major differences between the two are the fact that the valueExpression should return a boolean and the fact that the edit variable is a boolean. For example, to display the "abstract" status of an EClass, we could use the following value expression:

* feature:abstract
* aql:self.abstract

And to edit this value, we could use the expression:

* aql:self.eSet(self.eClass().getEStructuralFeature('abstract'), arg0)

In this scenario, we would also need a way to edit the value of a structural feature in a more compact manner.

###### Radio

The concept of Radio widget requires a proposal expression which will return the list of proposal to display in the user interface. Those proposals will be displayed using a proposal label expression. The proposal selected will be determined by the value expression. In order to edit the semantic model, the edit expression will be called with the edit variable containing the proposal selected.

In order to display a choice between several page for a view to select the default page, we could use the following proposals expression:

* aql:self.eResource().eAllContent(Page) // Highly unoptimized

In order to display those Pages retrieved, we could use the following proposal label expression:

* aql:'Page '.concat(self.identifier)

To set the existing value, we could use the value expression:

* feature:page

And to edit this value, we could use:

* aql:self.setPage(arg0)

###### Select

The concept of the Select widget will require a boolean indicating if the Select widget should allow a single value selected or multiple values selected. This widget would have a behavior similar to the Radio widget. As a result, to display the list of EClass which could act as the supertype of a current EClass, we could use the following proposals expression:

* aql:self.eResource().eAllContents(EClass).excluding(self) // Highly unoptimized

To display those results, we could use the following proposal label expression:

* feature:name
* aql:'EClass '.concat(self.name)


In order to retrieved the supertypes of the current EClass, we could use the value expressions:

* feature:supertypes
* aql:self.supertypes

In order to edit the current value, we could use the expressions:

* aql:self.eSet(self.eClass().getEStructuralFeature('supertypes'), arg0)

We have to consider that if the Select widget has its field multiple set to false, arg0 will contain an object, if multiple is set to true, arg0 will contain a collection.

###### Cell Widget

The concept of Cell widget is used to identify the different kind of widgets that can be used in the cell of a table.

###### Widgets Model

{% highlight java linenos %}
@Ecore(nsURI="http://www.eclipse.org/eef/core/widgets/1.0.0", nsPrefix="eef-core-widgets")
package org.eclipse.eef.core.api.widgets

abstract class Widget {
  String identifier
}

class Variable {
  String name
}

abstract class CellWidget extends Widget {}

class Text extends CellWidget {
  String valueExpression
  String editExpression
  contains Variable editVariable
}

class Checkbox extends CellWidget {
  String valueExpression
  String editExpression
  contains Variable editVariable
}

class Radio extends Widget {
  String proposalsExpression
  String proposalLabelExpression
  String valueExpression
  String editExpression
  contains Variable editVariable
}

class Select extends CellWidget {
  boolean multiple
  String proposalsExpression
  String proposalLabelExpression
  String editExpression
  contains Variable editVariable
}

{% endhighlight %}
