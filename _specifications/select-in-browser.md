---
layout: specification
description: Select in browser
---

##### User stories
The purpose of this feature is to get the semantic element associated to a widget selected in the model explorer.

###### US-1 As an end user, Lucas wants to get the corresponding element in the model explorer selected when he set the focus on a widget in the properties view

Lucas is using a Sirius based modeler and he selects a diagram element, automatically the properties view shows the information associated to this element. When Lucas selects an element in the properties view, the element is automatically selected.

##### Proposed solutions

Following we present the different ideas we had for the behavior of this feature:

###### Buttons on widgets

First idea was to provide a button widget which when it will be clicked select the corresponding semantic element in the model explorer. This button will be associated to a java service that will do the selection in the model explorer. This means that the service should be able to get access to the currently selected semantic element.

--> Horrible User eXperience, too much buttons

###### Buttons on sections

Add a button on a section. When this button will be clicked, it will select the associated semantic element in the model explorer. This means that we should be able in the EEF DSL to provides a button description on a section and a behavior associated to this button. A semantic candidate expression will be also available to get the associated semantic element.

![Buttons on sections]({{ site.baseurl }}specifications/images/select-in-browser/SelectInBrowserButtonOnSection.png "Buttons on sections")

--> Can not select one by one the semantic elements of a multiple references.

###### Focus

Add a toggle button "Link with browser" at the bottom of the properties view. If this button is activated, when the user set the focus on a field in the properties view, the EEF runtime selects the associated semantic element in the model explorer. This solution means that we should be able to customize the semantic element that must be associated to the focus. This means that we should integrate a `onSelectionExpression` in the EEF DSL which returns the element that must be selected in the browser on the set focus action. 

![Focus]({{ site.baseurl }}specifications/images/select-in-browser/SelectInBrowserFocus.png "Focus")

--> Must activate / deactivate the feature for big models and too much clicks when we want to activate the feature, see a multiple reference in the browser and deactivate the feature.

####### Hyperlinks

Another solution could be to use the hyperlink widget. An hyperlink widget has an `onClickExpression`, the the specifier uses this expression to call a Java service which selects the clicked semantic element in the browser.

--> Seems to be a good solution nothing else to code than being sure to use hyperlinks in the single/multiple references widgets. See the [complex widgets specification]({{ site.baseurl }}specifications/complex-widgets.html "complex widgets specification").

###### Code

To select an element in the model explorer, use classical navigator API.

{% highlight java linenos %}
IViewPart modelExplorerView = null;
			try {
				modelExplorerView = PlatformUI.getWorkbench().getActiveWorkbenchWindow().getActivePage()
						.showView("org.eclipse.sirius.ui.tools.views.model.explorer");
				if (modelExplorerView instanceof CommonNavigator) {
					final CommonNavigator modelExplorer = (CommonNavigator)modelExplorerView;
					modelExplorer.selectReveal(new StructuredSelection(selectedObject));
				}
			} catch (final PartInitException e) {
				// Catch exception
			}
{% endhighlight %} 