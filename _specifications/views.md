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

# PREVIOUS API!!!

###### Editing Model

{% highlight java linenos %}
@Ecore(nsURI="http://www.eclipse.org/emf/eef/editingmodel/1.0.0", nsPrefix="eef-model")
package org.eclipse.emf.eef.runtime.editingModel

import org.eclipse.emf.ecore.EObject
import org.eclipse.emf.eef.runtime.query.Filter

class PropertiesEditingModel {
  String id
  String name
  refers EObject[] involvedModels
  contains EClassBinding[] bindings opposite editingModel
  contains EditingOptions options

  op EClassBinding binding(EObject eObject) {}
  op View[] views(EObject eObject) {}
  op EClassBinding bindingEClass(EClass eClass) {}
}

class EClassBinding {
  refers EClass eClass
  contains View[1..*] views
  contains PropertyBinding[] propertyBindings
  container PropertiesEditingModel editingModel opposite bindings

  op EJavaObject propertyEditor(EObject eObject,
                                EStructuralFeature eStructuralFeature,
                                EBoolean autowire)
  op PropertyBinding propertyBinding(EJavaObject eJavaObject,
                                     EBoolean autowire)
}

abstract class View {}

abstract class Editor {}

class PropertyBinding {
  contains Editor editor
  contains PropertyBinding[] subPropertyBindings
  contains EditorSettings[] settings
  String bindingCustomizer

  op EClassBinding getEClassBindings() {}
}

class EStructuralFeatureBinding extends PropertyBinding {
  refers EStructuralFeature feature
}

class JavaView extends View {
  refers EJavaObject definition
}

class EObjectView extends View {
  refers EObject definition
}

class JavaEditor extends Editor {
  refers EJavaObject definition
}

class EObjectEditor extends Editor {
  refers EObject definition
}

class EditingOptions {
  refers FeatureDocumentationProvider featureDocumentationProvider
}

enum FeatureDocumentationProvider {
  GenmodelPropertyDescription = 0
  EcoreDocumentation = 1
}

class EditorSettings {}

class EReferenceFilter extends EditorSettings, Filter {}

type Void wraps java.lang.Void
{% endhighlight %}


###### Query Model

{% highlight java linenos %}
@Ecore(nsURI="http://www.eclipse.org/emf/eef/query/1.0.0", nsPrefix="eef-query")
package org.eclipse.emf.eef.runtime.query

import org.eclipse.emf.ecore.EObject
import org.eclipse.emf.ecore.EBooleanObject

class Query<TYPE> {
  contains Body body
}

class Body {}

class JavaBody extends Body {
  String bundle
  String qualifiedClass
  String method
}

class Filter extends Query<EBooleanObject> {}

class Navigation extends Query<EObject> {}

type ClassLoader wraps java.lang.ClassLoader
{% endhighlight %}

###### Views Model

{% highlight java linenos %}
@Ecore(nsURI="http://www.eclipse.org/emf/eef/views/1.0.0", nsPrefix="eef-views")
package org.eclipse.emf.eef.views

import org.eclipse.emf.eef.views.toolkits.Widget

class ViewsRepository extends DocumentedElement, IdentifiedElement {
  String name
  String[] RepositoryKind
  contains View[] views opposite repository
  contains Category[] categories
}

class View extends Container, IdentifiedElement {
  Boolean explicit
  container ViewsRepository repository opposite views
  container Category category opposite views
}

class ElementEditor extends ViewElement, IdentifiedElement {
  Boolean readOnly
  Boolean nameAsLabel
  contains ElementEditor[] subElementEditors
}

class Category extends DocumentedElement {
  String name
  contains View[] views opposite category
  contains Category[] categories
  container ViewsRepository repository opposite categories
}

class Container extends ViewElement {
  contains ViewElement elements opposite container
}

class ViewElement extends DocumentedElement {
  String name
  refers Widget representation
  container Container container opposite elements

  op View getContainingView() {}
}

class CustomElementEditor extends ElementEditor {}

class CustomView extends View {}

class DocumentedElement {
  String documentation
}

class ViewReference extends ViewElement, IdentifiedElement {
  refers ViewElement view
}

class IdentifiedElement {
  String qualifiedIdentifier
}
{% endhighlight %}
