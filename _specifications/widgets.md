---
layout: specification
description: Widgets DSL
---

We need to be able to define a toolkit which will be used to create the user interface manipulated by the end users.

##### User Stories

###### Zoe - Sirius Specifier

As a Sirius specifier, Zoe wants to have a set of widgets that can be used in order to define how her models will be edited. Zoe, being used to Sirius will want to find concept similar to Sirius ones. She will want to have at least the following kind of widgets:

* text
* checkbox
* select (single and multiple)
* radio
* table

##### Functional Specification

A concept named Toolkit will hold all the widgets of the model. This concept only has an identifier and a list of widgets. Each widget has an identifier and it as an opposite reference to the Toolkit in order to navigate to it more easily.

###### Text

The concept of Text widget has two attributes. The first one, valueExpression, is an expression used to determine the value to be displayed in the user interface. This value can use the various interpreters available to be computed but it must return a String. For example:

* feature:name
* aql: self.firstName.concat(' ').concat(self.familyName)

The second attribute, editExpression is used to edit the semantic object. This expression would use the edit argument. The edit argument should be created automatically when the user creates a new Text widget. It should have by default the name arg0, this variable will always be a String. As such, the user could easily edit the semantic object using one expression like:

* aql: self.customAQLService(arg0)
* aql: self.eSet(self.eClass().getEStructuralFeature('firstName'), arg0)

While this approach offer some flexibility, some default service may be required in order to streamline the default use case. If an user want to have a simple mapping between a Text widget and an EString EAttribute, the value expression "feature:name" is very easy to setup but the edit expression is a bit more complicated. On the other side it would be very familiar for a Sirius specifier.

###### Checkbox

The concept of Checkbox is very similar to the Text one. The only major differences between the two are the fact that the valueExpression should return a boolean and the fact that the editArgument is a boolean.

###### Radio

The concept of Radio widget requires a proposal expression which will return the list of proposal to display in the user interface. The proposal selected will be determined by the value expression. In order to edit the semantic model, the edit expression will be called with the edit argument containing the proposal selected.

###### Widgets Model

{% highlight java linenos %}
@Ecore(nsURI="http://www.eclipse.org/eef/core/widgets/1.0.0", nsPrefix="eef-widgets")
package org.eclipse.eef.core.api.widgets

class Toolkit {
  String identifier
  contains Widget[] widgets opposite toolkit
}

abstract class Widget {
  String identifier
  container Toolkit toolkit opposite widgets
}

class Variable {
  String name
}

abstract class CellWidget extends Widget {}

class Text extends CellWidget {
  String valueExpression
  String editExpression
  contains Variable editArgument
}

class Checkbox extends CellWidget {
  String valueExpression
  String editExpression
  contains Variable editArgument
}

class Radio extends Widget {
  String[] proposalsExpression
  String valueExpression
  String editExpression
  contains Variable editArgument
}

class Select extends CellWidget {
  boolean multiple
  Object[] proposalsExpression
  String proposalLabelExpression
  String editExpression
}

class Table extends Widget {
  contains Column[] columns
  contains Line[] lines
}

class Column {
  String headerLabelExpression
  contains CellWidget cellWidget
}

class Line {
  String domainClass
  String semanticCandidatesExpression
  String headerLabelExpression
}

{% endhighlight %}
