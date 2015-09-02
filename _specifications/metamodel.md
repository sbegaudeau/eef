---
layout: specification
description: EEF Metamodel
---
The metamodel of EEF is used to specify the structure and behavior of the view to instantiate.

##### User stories

###### US-1 As a Sirius specifier, Zoe wants to create a Properties view

Zoe needs to define a Properties view for her Sirius designer. As the root concept of our EEF meta-model, we have define the concept of *View*. A *View* has an *identifier* in order to easily identify a specific *View* instance among all those available. The *View* has the list of the NsUris of the meta-models used. Since we do not want to have a direct reference to the meta-models used in order to not force their loading, we will only use the NsUri of the meta-models in a similar fashion as Eclipse Sirius. Those NsUris will be stored in a property named *ePackageNsUris*.

The label of the *View* can be easily defined using the property *labelExpression*. This property will contain an *expression* which will be interpreted using the available interpreters in the EEF runtime. Some of the interpreters currently used by Sirius specifiers will be available. This label of the *View* will be used as the label of the Eclipse's Workbench View when the EEF *View* is used an Eclipse View.

New concepts:

* View
* View#identifier
* View#ePackageNsUris
* View#labelExpression

###### US-2 As a Sirius specifier, Zoe wants to define tabs in a view to separate the different concerns

In order to show the various aspects of the objects that Zoe wants to edit, she wants to separate those various aspects using tabs. To represent those tabs, we will use the concept of *Page* in our meta-model. The *Page* needs to be identifiable using a property named *identifier*. The tab created by the *Page* needs to have a label to be identified by the end users. Zoo will be able to use the property *labelExpression*, in order to be able to compute this label easily.

Each *Page* will let the end users work on a different aspect of the input of the *View*. As a result, Zoe needs to be able to specify the context used as the root of the *Page*. Using a similar approach as Eclipse's Sirius, the *Page* will contain a *domainClass* and a *semanticCandidateExpression* in order to specify the EClass of the context of the *Page* and how this context should be computed from the input of the *View*.

Since a *Page* must have one and only one root context, the *semanticCandidateExpression* should return exactly one EObject.

New concepts:

* Page
* Page#identifier
* Page#labelExpression
* Page#domainClass
* Page#semanticCandidateExpression

###### US-3 As a Sirius specifier, Zoe wants to define parts of a tab to contain a set of widgets

In order to structure the user interface within a *Page*, Zoe needs to be able to create a group of widgets easily identifiable by the end users. To achieve this need, Zoe will be able to use the concept of *Group*. A *Group* contains an *identifier* in order to be easily identified in the *View*. This parts of the *Page* can have a label to be identifiable by the end users using a *labelExpression* just like the *Page*.

The various *Groups* within a *Page* may work on different concepts than the whole *Page*, as a result each *Group* can have its own context using the properties *domainClass* and *semanticCandidatesExpression*. Contrary to the property *semanticCandidateExpression* of the *Page*, this expression can return multiple EObjects. A *Group* will be created for each of those EObjects in the *Page*. This behavior allow us to define a *Group* displaying multiple sets of properties using each a different EObject as a context quite easily.

New concepts:

* Group
* Group#identifier
* Group#labelExpression
* Group#domainClass
* Group#semanticCandidatesExpression

###### US-4 As a Sirius specifier, Zoe wants factorize the definition of the *Groups*

In order to support the definition of the Properties view of a complex meta-model, Zoe needs to be able to factorize as much as possible of her work. In order to specify the way the properties of an EClass should be displayed, she does not want to have to specify once again how the properties of the superclass of said EClass. As an example, to display an instance of EAttribute, Zoe does not want to redefine how to display an EStructuralFeature, an ENamedElement and an EModelElement.

The concept of *Page* will not contain directly the various *Groups* available. They will be defined under the *View* directly and then the *Pages* will be able to reuse them easily. As such, the *Page* used to display the instances of EAttribute will be able to reuse the *Group* defined for the properties of the EStructuralFeature.

This approach creates an issue since that in order to display an EObject, the specifier would have to manually look for all the *Groups* related to its EClass and its parents under the *View*. To simplify this solution, if the *Groups* to use for the *Page* are not specifically referenced by the *Page*, the EEF runtime will look for all the *Groups* accessible under the *View* containing the *Page* and use them following the order of the inheritance tree of the EObject. As a result, by having a *View* containing four *Groups* for EAttribute, EstructuralFeature, ENamedElement and EModelElement, we could create a *Page* to display an EAttribute without specifying the *Groups* to reuse and the result would contain first, a *Group* showing the value of the properties from EModelElement, then a *Group* for the properties of ENamedElement, followed by a *Group* with the properties of EStructuralFeature and finally the *Group* showing the properties of EAttribute.

With the behavior introduced previously, we just have to create the *Groups* for our EClasses and the *Page* will automatically reuse them to display the various properties of all the EObjects using said EClasses. We can factorize easily the behavior of the properties of the inheritance tree. If Zoe wants to customize the *Groups* used for the *Page* of a specific EClass, she can manually reference the *Groups* used by a specific *Page*. With this she can also customize the order of appearance of the *Groups* for a specific *Page* since, if explicitly specified, the order to the *Groups* will follow the order defined in the property *groups* of the *Page*. It will also allow Zoe to prevent the use of specific *Groups* for a *Page* by not referencing them among the *Groups* explicitly referenced in the property *groups* of the *Page*.

New concepts:

* View#groups
* Page#groups

###### US-5 As a Sirius specifier, Zoe wants to reuse the *Groups* used for a specific *Page*

In the previous user story, we have defined how the *Page* will reuse the *Groups* automatically of as specified by the Sirius specifier. Now let's consider a scenario where Zoe has already customized the *Groups* used by some *Pages* for Ecore and now she wants to define the *Pages* for Genmodel but she wants to reuse the custom set of *Groups* defined in some existing *Pages*. In order to achieve this, Zoe can import other *Views* in the definition of her *View* using the property *importedViews* and she can

-> To not have to reimport everything, we will be able to extends Pages
-> To extend Pages of other View definitions, we can import other Views

New concepts:

* View#importedViews
* Page#extendedPage
