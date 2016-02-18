---
layout: specification
description: EEF properties view framework
---

#### Eclipse tabbed properties view framework

The generic _Properties_ view is implemented in `org.eclipse.ui.views.properties.PropertySheet`. It does mainly two things (of interest to us):
1. Listen to which parts (view or editors) are active on the workbench, and for each one determines how they want they properties displayed. This is done by calling `Adapters.adapt(part, IPropertySheetPage.class)` on each part of interest, and caching the resulting `IPropertySheetPage` instance. `IPropertySheetPage.createControl(Composite parent)` is the method in which the actual SWT widgets will be created.
2. Listen to the selection changes in the workbench (`PropertySheet` is an `org.eclipse.ui.ISelectionListener`). When the user selects an element in a part (view or editor), the workbench calls `selectionChanged(IWorkbenchPart part, ISelection  selection)` on all the registered `org.eclipse.ui.ISelectionListener`. When the `PropertySheet` receives this call, it enables the `IPropertySheetPage` corresponding to the specified part (creating it if necessary), and passes the selection to it via a similar `selectionChanged()` call.

One very commonly used implementation of `IPropertySheetPage` is `TabbedPropertySheetPage`, which exists in its own bundle, `org.eclipse.ui.views.properties.tabbed`. It is only one possible implementation of `IPropertySheetPage`. It defines its own mini-framework to structure the content of the property sheet in terms of pages (tabs) composed of sections, themselves made of widgets. Which concrete pages and sections are visible and in which order for a given selection is determined by the contributions registered through various extension points (`propertyContributor`, `propertyTabs`, `propertySection`).

This makes the framework very extensible, as it is possible for a plug-in to contribute new sections or tabs to an existing editor without changing its code. This is for example how Sirius transparently adds its own tabs to the diagram editor in addition to the ones defined by GMF Runtime, or how the tabs and sections of the VSM editor properties are a mix of elements coming from the core `viewpoint.ecore` and from dialect-specific properties like `diagram.ecore`.

This configuration mechanism based on extension points is however a very static approach, and this reflects in the structure of the `org.eclipse.ui.views.properties.tabbed` itself and the lifecycle it assumes (and imposes) on the tabs and sections. In the context of Sirius, users expect a much higher level of dynamicity, which could not be provided using `org.eclipse.ui.views.properties.tabbed`, so we created an alternative implementation named `org.eclipse.eef.properties.ui`. It is basically a fork of the existing framework, but much more dynamic:
- it does not rely on static information (in `plugin.xml`) to determine structure of the pages (still made of tabs and sections);
- the lifecycle it implements supports arbitrary changes in this structure at almost any point, which makes is possible for example for tabs to appear/disappear dynamically, something which was not possible with the existing framework.

TODO:
- explain IEEFTabDescriptorProvider and the extension point
- explain how to use this new framework instead of the old one
- explain that underneath it is still about pages (tabs) and sections
- explain the legacy bridge

#### EEF framework

The EEF tabbed property sheet page framework is a fork of the Tabbed property sheet page framework provided by the `org.eclipse.ui.views.properties.tabbed` plugin.
This framework is provided by the `org.eclipse.eef.properties.ui` plugin.

_Why we fork?_
_Main differences_ :

 * _Dynamic behavior_  
 * _Validation -> Forms_
 * _Java 5_
 * _Simplify Lifecycle_

##### EEF Tab descriptor provider extension point

The EEF tabbed property sheet page framework provide a new extension point **EEF Tab Descriptor Provider**.
This extension point is used to describe a list of tabs that will be contributed to the EEF tabbed property sheet page.

![Example of EEF Tab Descriptor Provider]({{ site.baseurl }}specifications/images/SiriusEEFTabDescriptorProvider.png "Example of EEF Tab Descriptor Provider")

This extension point requires :

* **id** : Identifier of the descriptor provider.
* **label** : Label of the descriptor provider.
* **description** : Description of the descriptor provider.
* **class** : Class implementing the `IEEFTabDescriptorProvider` interface to provide a list of `IEEFTabDescriptors` representing the tab descriptors which must be contributed to the EEF property view.

See the [Sirius properties UI section]({{ site.baseurl }}specifications/architecture/sirius/sirius-properties-ui.html) for more details about how this extension point is used in the Sirius' case. 

The `EEFTabbedPropertyRegistry` is in charge of getting all the tab descriptors contributed by the tabbed property extension points.
An extension registry listener `EEFTabDescriptorProviderRegistryEventListener` is used to populate the registry of tab descriptor providers.

##### Legacy support

In the legacy tabbed properties view framework, the tab descriptors are provided thanks to the extension points :

* `org.eclipse.ui.views.properties.tabbed.propertyContributor` : contributes categories
* `org.eclipse.ui.views.properties.tabbed.propertyTabs` : contributes tabs
* `org.eclipse.ui.views.properties.tabbed.propertySections` : contributes sections

The `org.eclipse.eef.properties.ui.legacy` plugin contributes to the EEF tabbed property view tab descriptors described thanks to the legacy tabbed properties view extension points.
It defines a `LegacyTabDescriptorProvider` which implements the `get` method of the `IEEFTabDescriptorProvider` interface. This method returns all the `IEEFTabDescriptor`.

The`LegacyPropertyTabRegistry` provides a `getPropertyTabs` method which :
* get the tabs provided by the legacy extension points : the `LegacyPropertyTabRegistry` is in charge of getting all the categories contributed by the legacy tabbed property extension points. The extension registry listener `LegacyPropertyTabsRegistryEventListener` is used to populate the registry. For each `propertyTab` encountered in the description, it creates a new `LegacyPropertyTabItemDescriptor`. This descriptor extends the `AbstractEEFTabDescriptor` class which computes all the sections contained by a tab by calling the `LegacyPropertySectionsRegistry#getPropertySections` method. The `LegacyPropertySectionRegistry` is in charge of getting all the categories contributed by the legacy tabbed property extension points. The extension registry listener `LegacyPropertySectionsRegistryEventListener` is used to populate the registry.
* get the categories provided by the legacy extension points : the `LegacyPropertyContributorRegistry` is in charge of getting all the categories contributed by the legacy tabbed property extension points. The extension registry listener `LegacyPropertyContributorRegistryEventListener` is used to populate the registry.
* sort the tabs according to the categories : Then as it was done in the legacy tabbed properties view framework, the tabs are sorted by there _categories_. 
* sort the tabs according to the after tab attribute: Finally as it was done in the legacy tabbed properties view framework, the tabs are sorted thanks to the _after tab_ attribute.

