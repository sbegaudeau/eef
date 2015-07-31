---
layout: specification
description: Views DSL
---
The View DSL of the EEF project should let us describe how the user interface will be structured. This DSL, similar in its goal to a Sirius odesign, will be connected to the concepts from the domain model of the user.

##### User stories

###### Zoe - Sirius Specifier

As a Sirius specifier, Zoe wants to describe a view for her domain. She wants to be able to define multiple pages for her views including a default page that the use will see first. She wants to define the use of multiple widgets in a page. Some widget should be able to contain child widgets in order to define an advanced layout. She also want to bind the concepts of EEF to some expressions which could query her domain models. Zoe does not want to link directly the EEF concepts to EStructuralFeatures or EClasses.

##### Functional Specification

Since EEF 1.x and EEF 2.x will need to be installable at the same time, EEF 2.x should not use the same namespace as EEF 1.X. As a result, the DSL describe here would be located in a bundle named org.eclipse.eef and more specifically in a package named org.eclipse.eef.api.views.

###### View

[![View Screenshot]({{ site.baseurl }}specifications/images/views/view-screenshot.png "View Screenshot")]({{ site.baseurl }}specifications/images/views/view-screenshot.png)

A View is the root concept in our DSL. This concept is not specifically linked with the Eclipse Workbench's concept view. The view has an identifier and as the root of our work, it will hold the information of the input. Those information will be separated in several parts:

* ePackageNsUris
* domainClass

The ePackageNsUris will hold the NsUris of all the meta-models that will be used by the View. We will not have direct references to the EPackage instances in order to prevent the compulsory loading of the meta-models. The domain class will be used to find the EClass of the element used as the input of the view.

A View will also contains a list of Pages used to display the various "layers" of Widgets to the end user. One of the Page can be defined as the default one. This Page will be presented to the user first when the View is opened.

###### Page

A Page holds the set of Widgets presented to the end user at a given moment. Like a View, a Page has an identifier in order to easily manipulate it.

[![Page Label Screenshot]({{ site.baseurl }}specifications/images/views/page-label-screenshot.png "Page Label Screenshot")]({{ site.baseurl }}specifications/images/views/page-label-screenshot.png)

The Page is also identified by the result of its label expression which will compute the label displayed to the end user (here, in the previous screenshot, "Base" or "Documentation").

A Page will be linked to a specific semantic element using the domain class and the semantic candidates expression. The page will also holds a list of Sections.

###### Section

A section is a area of the Page dedicated to a specific concern.

[![Section Screenshot]({{ site.baseurl }}specifications/images/views/section-screenshot.png "Section Screenshot")]({{ site.baseurl }}specifications/images/views/section-screenshot.png)

The Section will be linked to a specific semantic element using the domain class and the semantic candidates expression. It will also have a label expression in order to compute its label, for example "Properties" in the previous screenshot. The Section will contain a list of Mapping used to link the semantic elements with the Widgets used to display and/or edit their properties.

###### Mapping

Since there are various kinds of Mapping, their common super-type is an abstract class. The Mappings have at least a label expression and an identifier.

###### Container and Direction

The concept of Container will be used in order to structure the layout of the Widgets to use. A Container is a mapping which contain other Mapping and which has a direction. The Direction is an enumeration with two values VERTICAL or HORIZONTAL. An enumeration has been used since we don't plan on supporting other values for the Direction.

###### SemanticMapping

The Semantic Mapping is the concept liking the semantic elements and the Widgets.

[![Semantic Mapping Screenshot]({{ site.baseurl }}specifications/images/views/semantic-mapping-screenshot.png "Semantic Mapping Screenshot")]({{ site.baseurl }}specifications/images/views/semantic-mapping-screenshot.png)

For that, it uses a domain class and a semantic candidates expression in a similar fashion as Sirius. It also uses a Widget from the Widgets DSL in order to specify how the mapping will be displayed to the end user.

For more information on how the Widget is parameterized (value, edition, etc) have a look at the Widgets DSL.

###### Table, Column and Line

EEF also need to be able to display tables and as such, we need to be able to define them using the Views DSL. A Table is a regular Mapping with a domain class, a semantic candidates expression, a collection of columns and lines. A Line will be defined using a domain class indicating the kind of element used on the Line. As an example, we will display the content of an EPackage in a Table. The domain class of the table will thus be:

* ecore.EPackage

The semantic candidates expression could be:

* self.eResource().eAllContents(ecore::EPackage) // Highly unoptimized

Each line will represent an EClass and we will have two columns indicating if an EClass is abstract and the name of the EClass. First, we will use the following domain class for the line:

* ecore.EClass

We will use the same convention as Sirius using the prefix of the EPackage then a dot and then the name of the EClassifier. Then we will use a semantic candidates expression in order to retrieve all the values to display:

* aql:self.eClassifiers->filter(ecore::EClass)

We will also define a header label expression to define the label of the lines. If such expression is not available, the column with the label of the lines will not be displayed:

* feature:name

Then, we will need to define the column to display for the semantic elements retrieved. Each column will have a header label expression used for the label of the column, for example:

* Abstract
* aql:'Abstract EClass'

In order to display the data, in a cell, we will need to have a widget. For this need, several widget can extends the concept Cell widget which is used in order to identify the widgets that can be used within the cells of a Table. In the Widgets DSL, the following widgets can be used a cell widget:

* Text
* Checkbox
* Select

Thus in order to define the Checkbox used to display the status of the EAttribute abstract of the EClasses displayed, we could use a configuration similar to the one presented previously in this document.

###### Views Model

{% highlight java linenos %}
@Ecore(nsURI="http://www.eclipse.org/eef/views/2.0.0", nsPrefix="eef-views")
package org.eclipse.eef.api.views

import org.eclipse.eef.api.widgets.Widget

class View {
  String identifier
  String domainClass
  String[] ePackageNsUris
  refers Page defaultPage
  contains Page[] pages
}

class Page {
  String identifier
  String labelExpression
  String domainClass
  String semanticCandidatesExpression
  contains Section[] sections
}

class Section {
  String identifier
  String labelExpression
  String domainClass
  String semanticCandidatesExpression
  contains Mapping[] mappings
}

abstract class Mapping {
  String identifier
  Style labelExpression
}

class SemanticMapping extends Mapping {
  String domainClass
  String semanticCandidatesExpression
  contains Widget widget
}

class Container extends Mapping {
  Direction direction
  contains Mapping[] mappings
}

enum Direction {
  HORIZONTAL
  VERTICAL
}

class Table extends Mapping {
  String domainClass
  String semanticCandidatesExpression
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
