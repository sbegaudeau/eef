---
layout: specification
description: EEF properties view framework
---

#### Eclipse tabbed properties view framework

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

