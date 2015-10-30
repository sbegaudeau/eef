---
layout: specification
description: EEF Metamodel
---
The meta-model of EEF is used to specify the structure and behavior of the view to instantiate.

##### User stories

###### US-1 As a Sirius specifier, Zoe wants to create a Properties view

Zoe needs to define a Properties view for her Sirius designer using the EEF meta-model. As the root concept of our EEF meta-model, we have the concept of *View*. A *View* has an *identifier* in order to easily identify a specific *View* instance among all those available. The *View* has the list of the NsUris of the meta-models used. Since we do not want to have a direct reference to the meta-models used in order to not force their loading, we will only use the NsUri of the meta-models in a similar fashion as Eclipse Sirius. Those NsUris will be stored in a property named *ePackageNsUris* containing a list of Strings.

The label of the *View* can be easily defined using the property *labelExpression*. This property will contain an *expression* which will be interpreted using the available interpreters in the EEF runtime. Some of the interpreters currently used by Sirius specifiers will be available. This label of the *View* will be used as the label of the Eclipse's Workbench View when the EEF *View* is used an Eclipse View.

In order to compute the value of the expressions, some *Variables* will be accessible. As an example, the input of the View will be available using the *Variable* in the property *inputVariable* of the View. A *Variable* has only a name and it is used to store the name of the value that will be available in the expression. For example, using an EClass as an input, and using the *name* "self" for the *Variable* *inputVariable*, the label of the view could be computed to display the name of the input EClass with the *labelExpression*: "aql:self.name". This expression would rely on the interpreter "aql" and it would use the *Variables* provided by the EEF runtime for its context. For the evaluation of the *labelExpression*, we have here access to the input of the *View* under the name specified for the *inputVariable*. The name of the variable "self" used by our AQL expression here does not have any link whatsoever with a potential implicit variable. All the *Variables* will be referenced using an explicit name.

New concepts:

* View
* View#identifier: String [0..1]
* View#ePackageNsUris: String [0..-1]
* View#labelExpression: String [0..1]
* View#inputVariable: Variable [0..1] containment
* Variable
* Variable#name: String [0..1]

###### US-2 As a Sirius specifier, Zoe wants to define tabs in a view to separate the different concerns

In order to show the various aspects of the objects that Zoe wants to edit, she wants to separate those various aspects using tabs. To represent those tabs, we will use the concept of *Page* in our meta-model. The *Page* needs to be identifiable using a property named *identifier*. The tab created by the *Page* needs to have a label to be identified by the end users. Zoe will be able to use the property *labelExpression*, in order to be able to compute this label easily.

In our simple use case, our first *Page* will only show the properties of our input EClass. As a result, the label of our *Page* will be computed using the *labelExpression* "General". If we do not have any interpreter register that support specifically the expression typed by the user, we will handle the text entered by the user as a String. In our case, we will use the String "General" directly as a String.

Our *Page* needs to specify the context in which we will work using the property *domainClass*. This property does not referenced directly an EClass of a meta-model since we do not want to load said EClass directly in a similar approach as Eclipse Sirius. In our case, we are using the same context in the *Page* as the input of the *View* so Zoe will use the *domainClass* "ecore.EClass".

Zoe could use a different context by specifying a *semanticCandidateExpression* for the *Page*. By not specifying it, the EEF runtime will reuse the context of the *View* for the *Page*. This behavior would be equivalent to using "aql:self" as the *semanticCandidateExpression* of the *Page*.

New concepts:

* Page
* Page#identifier: String [0..1]
* Page#labelExpression: String [0..1]
* Page#domainClass: String [0..1]
* Page#semanticCandidateExpression: String [0..1]

###### US-3 As a Sirius specifier, Zoe wants to define parts of a *Page* to contain a set of widgets

In order to structure the user interface within a *Page*, Zoe needs to be able to create a group of widgets easily identifiable by the end users. To achieve this need, Zoe will be able to use the concept of *Group*. A *Group* contains an *identifier* in order to be easily identified in the *View*. This parts of the *Page* can have a label to be identifiable by the end users using a *labelExpression* just like the *Page*.

If you use a *labelExpression* for your *Group* it will create in the user interface a complete section with its label visible. If you do not want to see the *Group* as a visible structure in the resulting user interface, do not specify any *labelExpression*. As a result, the boundaries of the *Group* will not be visible in the result, it will be mainly used as a layout structure. Using a similar approach as before, we will create a label for our *Group* using the String "ENamedElement" which will be used directly as is.

Just like the *Page*, the *Group* needs to have a *domainClass* in order to specify the context in which this *Group* will be used. In order to work with an ENamedElement, the *domainClass* will be "ecore.ENamedElement". The *semanticCandidatesExpression* will stay empty to keep working using the context of the *Page* where the *Group* will be used.

New concepts:

* Group
* Group#identifier: String [0..1]
* Group#labelExpression: String [0..1]
* Group#domainClass: String [0..1]
* Group#semanticCandidatesExpression: String [0..1]

###### US-4 As a Sirius specifier, Zoe wants factorize the definition of the *Groups*

In order to support the definition of the Properties view of her meta-model, Zoe needs to be able to factorize her work as much as possible. As such, if she has specified how to display a subset of the properties of an EClass, she does not want to have to redefine everything when she will work on displaying one of its subclass.

The concept of *Page* will not contain directly the various *Groups* available. They will be defined under the *View* directly and then the *Pages* will be able to reuse them easily. As such, the *Page* used to display the instances of EClass should be able to reuse a *Group* defined for the properties of ENamedElement. Zoe will be able to define a *Group* for the properties of ENamedElement and this *Group* will be reused for all the subclasses of ENamedElement. This approach creates an issue since that in order to display an EObject, the specifier would have to manually look for all the *Groups* related to its EClass and its parents under the *View*.

In her case, Zoe will create a second *Group* using the *labelExpression* "EClass" for the *domainClass* "ecore.EClass". Since the *Page* uses the *domainClass* "ecore.EClass", the *Page* will be able to work with both *Groups*. If the *Groups* to use for the *Page* are not specifically referenced by the *Page*, the EEF runtime will look for all the *Groups* accessible under the *View* containing the *Page* that have a *domainClass* which is equal or a superclass of the *domainClass* of the *Page* and use them following the order of the inheritance tree of the EObject.

In our case, the *Page* has the *domainClass* "ecore.EClass" and the *View* contains two *Groups*, one for "ecore.ENamedElement" and the other for "ecore.EClass" so since the *Page* does not explicitly reference any *Group* the runtime will automatically import both *Groups* in the *Page* and it will order them using the inheritance order. As such, since EClass extends ENamedElement, the first *Group* will be the one named "ENamedElement" and the second one will be the one named "EClass".

If Zoe wants to customize the *Groups* used for the *Page* of a specific EClass, she can manually reference the *Groups* used by a specific *Page*. With this she can also customize the order of appearance of the *Groups* for a specific *Page* since, if explicitly specified, the order to the *Groups* will follow the order defined in the property *groups* of the *Page*. It will also allow Zoe to prevent the use of specific *Groups* for a *Page* by not referencing them among the *Groups* explicitly referenced in the property *groups* of the *Page*.

New concepts:

* View#groups: Group [0..-1] containment
* Page#groups: Group [0..-1]

###### US-5 As a Sirius specifier, Zoe wants to add a text *Widget* to display and edit the name of an ENamedElement

We have already seen how the *View* is structured using *Pages* and *Group*, now Zoe wants to add her first widget to display the name of the EClass received as the input of the *View*. This name will be displayed in the *Group* used for the properties of ENamedElements. As a first step, she has to create a *Container* which will be used to hold the widgets of the *Group* for the ENamedElement *Group*. A *Container* needs to be identified and as such it contains a property *identifier*.

In order to hold the widgets, the *Container* will use a property named *widgets*. All the widgets that can be used in a *Container* will extend a common superclass named *Widget*. All the *Widgets* will have an *identifier* and a label computed using a *labelExpression*. In order to display the name of the EClass received as the input of the *View*, Zoe can use a specific *Widget*, the *Text*.

A *Text* is a subclass of *Widget* which will be used to create a text field in the user interface. This text field will be used to display and edit a value. In our use case, this *Text* will be used to edit an EAttribute of type EString but in a similar fashion as Eclipse's Sirius, we could use this field to edit another kind of value. This *Text* will first use the *labelExpression* "Name" in order to indicate to the end users that the field created will be used to edit the name of the EClass. Zoe needs to display the current value of the name of the EClass received as the input of the *View* and to achieve this she can use the *valueExpression* of the *Text* in order to indicate how the value displayed should be computed. We know that the input of the *View* is accessible using the *inputVariable* and since Zoe has named her *inputVariable* "self", we can use the *valueExpression* "aql:self.name" to display the name of the EClass in the *Text*.

In order to edit the value of the name of the input EClass, Zoe can use the *editExpression* of the *Text*. This *editExpression* will be executed when the end users will enter some text in the text field. In order to know how to access the value entered by the end users, Zoe will use the property *editVariable* to define the name of the *Variable* which will contain this value. In our case, Zoe will create an *editVariable* with the name "newName" and thus, she could write the following *editExpression* to edit the name of the EClass "aql:self.eSet(self.eClass().getEStructuralFeature('name'), newName)".

Now Zoe has created a *Text* which can be used to display and edit the name of the EClass. This approach is directly inspired by the way a Label and a Direct Edit Tool work in Eclipse Sirius.

New concepts:

* Container
* Container#identifier: String [0..1]
* Container#widgets: Widget [0..-1] containment
* Widget
* Widget#identifier: String [0..1]
* Widget#labelExpression: String [0..1]
* Text
* Text#valueExpression: String [0..1]
* Text#editExpression: String [0..1]
* Text#editVariable: Variable [0..1] containment

###### US-6 As a Sirius specifier, Zoe wants to add a checkbox *Widget* to display and edit the property abstract of an EClass

Now that we have seen how to use a *Text*, using a *Checkbox* will feel easy. This *Checkbox* will be used to represent an attribute specific to an EClass so it will be defined in the EClass *Group*. In this *Group*, Zoe will first define a *Container* then Zoe will create a *Checkbox* using "Abstract" for its *labelExpression* since it extends *Widget*. In order to display the property abstract of the input EClass, Zoe can use the *valueExpression* of the *Checkbox* with the expression "aql:self.abstract". In a similar fashion as the *Text*, to edit the value of the property abstract, Zoe can define an *editVariable*. Here, the *editVariable* of the *Checkbox* will have the *name* "newAbstract". After that, Zoe can define the *editExpression* of the *Checkbox* using the expression "aql:self.eSet(self.eClass().getEStructuralFeature('abstract'), newAbstract)".

As usual, just like in Sirius it is possible using a different expression to edit something completely unrelated to a boolean using a *Checkbox*.

New concepts:

* Checkbox
* Checkbox#valueExpression: String [0..1]
* Checkbox#editExpression: String [0..1]
* Checkbox#editVariable: Variable [0..1] containment

###### US-7 As a Sirius specifier, Zoe wants to add a label *Widget* to display some read only information.

Zoe can use a *Label* in order to add a read only piece of text in the *Container*. In the *Container* of the first *Group*, Zoe will add a *Label* with the *labelExpression* "Description". The body of the label will contain in our case a piece of static text (not a computed one) using the *valueExpression* "This is the label description".

New concepts:

* Label
* Label#valueExpression: String [0..1]

###### US-8 As a Sirius specifier, Zoe wants to add a hyperlink in order to execute an action

Zoe can create a *Link* in the *Container* of her first *Group* in order to display a label with an action. The label of the *Link* will be computed using the property *labelExpression* since *Link* extends *Widget*. When the user will click on the *Link*, the behavior specified in the property *onClickExpression* will be executed. Here Zoe will use the expression "Open documentation" as the *labelExpression* of the *Link* and "aql:self.openDocumentation()" for the *onClickExpression*.

The service "openDocumentation" does not exist on an ENamedElement neither on EClass so Zoe will need to contribute it to this definition. For that, she can use the concept of *JavaExtension* in a similar manner as in Eclipse Sirius. A *JavaExtension* can be created in the *View* using the property *javaExtensions* and it should contain a *qualifiedName*. In our case, Zoe can create a *JavaExtension* with the *qualifiedName* "org.eclipse.eef.myview.Service". Zoe will have to create the Java class with the same qualified name in the project containing her EEF *View* model and in this class, she will have to create a method with the following signature:

{% highlight java linenos %}
public class Service {
  public void openDocumentation(ENamedElement eNamedElement) {
    // insert here the code used to open a new editor with the documentation
  }
}
{% endhighlight %}

This service will be registered and available for the various interpreters. With the AQL interpreter, this service will be register as a regular service available on all ENamedElements. When the end user will click on the *Link*, the method openDocumentation will be called.

New concepts:

* Link
* Link#onClickExpression: String [0..1]
* JavaExtension
* JavaExtension#qualifiedName: String [0..1]
* View#javaExtensions: JavaExtension [0..-1] containment

###### US-9 As a Sirius specifier, Zoe wants to add an image to her *Group*.

In order to display an image, Zoe can add to a *Container* an *Image*. The *Image* will display an image using the URI computed from its *valueExpression*. In our example, Zoe will use "aql:'platform:/plugins/org.eclipse.emf/icons/obj16/EClass.gif'". The URI entered by the specifier will require a scheme, here "platform". By using this scheme, the EEF runtime will consider the URI as an EMF URI and it would look for the image in the plugin "org.eclipse.emf".

New concepts:

* Image
* Image#valueExpression: String [0..1]

###### US-10 As a Sirius specifier, Zoe wants to display and edit an image

We have seen how Zoe can display an *Image* in her *Group* but if she wants to let the end user edit the image in question, she can use an *ImagePicker*. The *ImagePicker* is a *Widget* which can display and image and let the end user modify it. To display an image, Zoe can use the *valueExpression* of the *ImagePicker*. This *valueExpression* will have the same behavior as the *valueExpression* of the *Image*.

The end user will need to select a new image among a collection of images. This collection will be computed using the URIs returned by the *candidatesExpression*. In order to display those images, Zoe will use the expression "aql:self.getCandidateImages()". To update the value of the image, Zoe will use an *editVariable* with the name "newImage" and an *editExpression* using the expression "aql:self.updateImage(newImage)". This expression will also use a method of our Java extension.

New concepts:

* ImagePicker
* ImagePicker#valueExpression: String [0..1]
* ImagePicker#candidatesExpression: String [0..1]
* ImagePicker#editVariable: Variable [0..1] containment

###### US-11 As a Sirius specifier, Zoe wants to display and edit a value using a radio *Widget*

In order to use a radio *Widget*, Zoe can use a *Radio*. For that, Zoe needs to start by specifying the various proposals available using the property *candidatesExpression*. In our case, Zoe will enter the expression "aql:Sequence{'public', 'protected', 'private'}". Those proposals will not be displayed as is in the user interface. To determine how the proposals will be displayed, Zoe will use the property *candidateDisplayExpression" along with the property *candidateVariable*. The *candidateVariable* will use the name "visibility" and "aql:visibility + 'class'" for the *candidateDisplayExpression*. As a result, the three proposals will be "public class", "protected class" and "private class".

In order to initialize the value of the *Radio*, Zoe will have access to the property *valueExpression*. This expression will be used to select on the existing candidate returned by the *candidatesExpression*. In this situation, the *valueExpression* will be "aql:self.getVisibility()". When the end user will select a value, the expression defined in the property *editExpression* will be executed. This expression will have access to the *Variable* in the property *editVariable*. In our use case, the *editVariable* will be named "newVisibility" and the *editExpression* will be "aql:self.updateVisibility(newVisibility)".

New concepts:

* Radio
* Radio#candidatesExpression: String [0..1]
* Radio#candidateVariable: Variable [0..1] containment
* Radio#candidateDisplayExpression: String [0..1]
* Radio#valueExpression: String [0..1]
* Radio#editVariable: Variable [0..1] containment
* Radio#editExpression: String [0..1]

###### US-12 As a Sirius specifier, Zoe wants to display and edit some text in a text area.

We have previously define how to edit some text in a *Text*. In order to display the value in a text are instead, we can change the property *lineCount* of the *Text*. By default, this property has the value 1. If the *lineCount* is an integer bigger than 1, a text area will be used instead of a text field in the user interface. A *Text* could be used to display the documentation of the input EClass using "aql:self.getDocumentation()" as the *valueExpression*. This documentation could be updated using an *editVariable* named "newDocumentation" and the *editExpression* "aql:self.updateDocumentation(newDocumentation)". This *Text* could use a *lineCount* of 4.

New concepts:

* Text#lineCount: String [0..1]

###### US-13 As a Sirius specifier, Zoe wants to edit a value with multiple choices using a select box.

The EEF meta-model comes with a concept of *Select*, a *Widget* used to display a collection of candidate used to edit a single or multi-valued property. In her use case, Zoe wants to edit the superclasses of her input EClass. For that, Zoe will create a *Select* with *multiple* equal to true and using "aql:self.eSuperTypes" as the *valueExpression*. In order to find out all the possible candidates, Zoe can used an *candidatesExpression* with "aql:self.eResource().eAllContents(ecore::EClass)". This expression will return all the EClass in the same resource as the input EClass. In order to display them, Zoe can use a *candidateVariable* named "superType" and "aql:superType.name" as a *candidateDisplayExpression*.

To edit the value, Zoe can use an *editVariable* named "newSuperTypes" and "aql:self.updateSupertypes(newSuperTypes)" as an *editExpression*.

New concepts:

* Select
* Select#multiple: Boolean [0..1]
* Select#valueExpression: String [0..1]
* Select#candidatesExpression: String [0..1]
* Select#candidateVariable: Variable [0..1] containment
* Select#candidateDisplayExpression: String [0..1]
* Select#editVariable: Variable [0..1] containment
* Select#editExpression: String [0..1]

###### US-14 As a Sirius specifier, Zoe wants to let the user select a value in a tree.

In order to let the end users select a value in a tree, Zoe will first define the description of a tree and then use it in the *Widget*. As such, the tree described can be reused in multiple *Widgets* after that. In her use case, Zoe will create a *TreeDescription* showing the input EClass and its content. The *TreeDescription* will allow her to select a set of EObjects related to the input EClass. The *TreeDescription* will be created in the property *treeDescriptions* of the *View*.

The *TreeDescription* is an abstract concept with two different kinds of *TreeDescription*. The *InterpretedTreeDescription* which will define the structure of the *Tree* using various expressions and *Variables* and the *AdapterFactoryTreeDescription* which will let the specifier create a *Tree* initialized from an AdapterFactory. In this use case, Zoe will work with an *InterpretedTreeDescription*. After having set its *identifier* and *multiple* to true, both properties are defined on *TreeDescription*.

As the first step, Zoe will use the *containerSemanticCandidateVariable* to hold the value of the context of its *Container*. This *Variable* will be named "containerContext". Zoe will use the property *rootsExpression* to define the collection of elements that should appear as the root of the *Tree*. In her case, she will use the expression "aql:containerContext" to have the current EClass as the root. To compute the child to display in the *Tree*, Zoe can use "aql:parent.eContents()" as an *childrenExpression* along with a *parentVariable* named "parent". To display those elements in the tree, Zoe can use "aql:candidate.name" as an *candidateDisplayExpression* and "candidate" as the name of the *candidateVariable*. Finally, to determine the elements that can be selected, Zoe can provide "aql:candidate.ancestors().contains(containerContext)" as the *selectablePredicateExpression*.

In order to use the *TreeDescription*, Zoe will have to create a *Tree*. This *Tree* will first referenced the existing *TreeDescription*, then she will set the *valueExpression* to the expression "aql:self.getRelatedObjects()" in order to return the related Objects that should be selected in the user interface. In order to edit the value of the related Objects when the user modify the selection, the value will be captured using an *editVariable* named "selection" and an *editExpression* "aql:self.updateRelatedObjects(selection)".

New concepts:

* TreeDescription
* TreeDescription#identifier: String [0..1]
* TreeDescription#multiple: Boolean [0..1]
* View#treeDescriptions: TreeDescription [0..-1] containment
* InterpretedTreeDescription
* InterpretedTreeDescription#containerSemanticCandidateVariable: Variable [0..1]
* InterpretedTreeDescription#rootsExpression: String [0..1]
* InterpretedTreeDescription#childrenExpression: String [0..1]
* InterpretedTreeDescription#parentVariable: Variable [0..1] containment
* InterpretedTreeDescription#candidateDisplayExpression: String [0..1]
* InterpretedTreeDescription#candidateVariable: Variable [0..1] containment
* InterpretedTreeDescription#selectablePredicateExpression: String [0..1]
* AdapterFactoryTreeDescription
* Tree
* Tree#treeDescription: TreeDescription [0..1]
* Tree#valueExpression: String [0..1]
* Tree#editVariable: Variable [0..1] containment
* Tree#editExpression: String [0..1]

###### US-15 As a Sirius specifier, Zoe wants to reuse a *TreeDescription* to display a dialog

In order to create a dialog showing a tree, Zoe can use the concept of *TreeDialogSelect* which can leverage an existing *TreeDescription*. Contrary to the *Tree*, the *TreeDialogSelect* can change its context using the property *domainClass* and a *semanticCandidateExpression* with "ecore.EClass" as the *domainClass* and "aql:self" as the *semanticCandidateExpression*.

New concepts:

* TreeDialogSelect
* TreeDialogSelect#titleExpression
* TreeDialogSelect#descriptionExpression
* TreeDialogSelect#treeDescription
* TreeDialogSelect#valueExpression
* TreeDialogSelect#editExpression
* TreeDialogSelect#editVariable
* TreeDialogSelect#defaultFilter
* TreeDialogSelect#domainClass: String [0..1]
* TreeDialogSelect#semanticCandidateExpression: String [0..1]


###### US-16 Table

###### US-17 As a Sirius specifier, Zoe wants to reuse the *Groups* used for a specific *Page*

In the previous user story, we have defined how the *Page* will reuse the *Groups* automatically of as specified by the Sirius specifier. Now let's consider a scenario where Zoe has already customized the *Groups* used by some *Pages* for Ecore and now she wants to define the *Pages* for Genmodel but she wants to reuse the custom set of *Groups* defined in some existing *Pages*. In order to achieve this, Zoe can import other *Views* in the definition of her *View* using the property *importedViews* and she can then extend an existing *Page* to retrieve the specific configuration of the *Groups* of the *Page*. She can extends the set of *Groups* used in the extended *Page* by specifying new *Groups* to use additionally in her *Page*.

As such, she can define a *Page* for the concept GenClass which can contain a *Group* for the properties of GenClass and which extends an existing *Page* defined in another *View* to display EClass. This *Page* used to display an EClass had already defined a set of *Groups* that should be displayed in a specific order using for example a *Group* for EClass followed by a *Group* for EClassifier. As a result, the final *Page* would have firstly the *Group* for EClass, followed by the *Group* for EClassifier and finally the *Group* for GenClass.

New concepts:

* View#importedViews
* Page#extendedPage


####### US-18 One widget for n structural feature


####### US-19 Complex example for semanticCandidatesExpression and domainClass


GROUP#PAGE SEMANTIC CANDIDATE VARIABLE


####### US-20 Layout multi columns


####### Properties View activated when we are on a specific mapping

####### Properties View activated when we are using an EObject from the Model Explorer view, a Representation or a layer


Additional user stories
* tree table
* how to represent the customization and when should they be activated?
* how to decide when a tab / a widget / a set of widget should be refreshed?
* rename the widgets in WText, WLabel, WCheckbox, etc?
* live preview editor as in Sirius
