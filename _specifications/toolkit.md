---
layout: specification
description: Widgets DSL
---

We need to be able to define a toolkit which will be used to create the user interface manipulated by the end users.

##### User Stories

###### Claire - EEF Developer

As an EEF developer, Claire wants to define the various kind of widgets available in EEF.

##### Functional Specification

###### Widgets Model

{% highlight java linenos %}
@Ecore(nsURI="http://www.eclipse.org/eef/core/widgets/1.0.0", nsPrefix="eef-widgets")
package org.eclipse.eef.core.widgets

class Toolkit {
  String identifier
  contains Widget[] widgets opposite toolkit
}

class Widget {
  String identifier
  container Toolkit toolkit opposite widgets
}

{% endhighlight %}
