---
layout: specification
description: Views DSL
---
The View DSL of the EEF project should let us describe how the user interface will be structured. This DSL, similar in its goal to a Sirius odesign, will be connected to the concepts from the domain model of the user.

##### User stories

###### Zoe - Sirius Specifier

As a Sirius specifier, Zoe wants to describe a view for her domain. She wants to be able to define multiple pages for her views including a default page that the use will see first. She wants to define the use of multiple widgets in a page. Some widget should be able to contain child widgets in order to define an advanced layout. She also want to bind the concepts of EEF to some expressions which could query her domain models. Zoe does not want to link directly the EEF concepts to EStructuralFeatures or EClasses.

##### Functional Specification

Since EEF 1.x and EEF 2.x will need to be installable at the same time, EEF 2.x should not use the same namespace as EEF 1.X. As a result, the DSL describe here would be located in a bundle named org.eclipse.eef.core and more specifically in a package named org.eclipse.eef.core.api.views.

###### Views Model

[![Screenshot]({{ site.baseurl }}specifications/images/views/screenshot.png "Screenshot")]({{ site.baseurl }}specifications/images/views/screenshot.png)

{% highlight java linenos %}
@Ecore(nsURI="http://www.eclipse.org/eef/core/views/2.0.0", nsPrefix="eef-core-views")
package org.eclipse.eef.core.api.views

import org.eclipse.eef.core.api.widgets.Widget

class View {
  String identifier
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
  Style labelExpression
}

class SemanticMapping extends Mapping {
  String domainClass
  String semanticCandidatesExpression
  refers Widget widget
}

class Container extends Mapping {
  Direction direction
  contains Mapping[] mappings
}

enum Direction {
  HORIZONTAL
  VERTICAL
}

{% endhighlight %}
