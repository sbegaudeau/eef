---
layout: meetingminutes
title: 2015-07-21
description: Preparation of the first technical meeting
---
##### Goal

The goal of this meeting is to realize an analysis of EEF in order to come up with some questions for the first technical meeting.

##### Participants

* Mélanie Bats
* Stéphane Bégaudeau

##### General

1. An instance of Gerrit is necessary
2. The election process for Mélanie and Stéphane should be launched in order to have access to some tools on the project.
3. A valid target platform is needed. There are two target platforms in EEF and both of them are not complete enough to make the whole code compile.
4. Is EEF only for Properties view?
5. Does EEF work with only and EObject and a SWT Composite?
6. Does it require EObject to be in a Resource? In a ResourceSet?

##### Dependencies

1. Does EEF uses Guava?
2. Which version?
3. Is Guava visible in the API?
4. The runtime bundle has a dependency to org.eclipse.core.runtime for the log
5. Dependency to the ResourceSet in EditingModelEnvironment.getResourceSet()
6. Dependency to the EditingDomain in DomainAwarePropertiesEditingContext.getEditingDomain()
7. Why does ViewChangeNotifier extends java.beans.PropertyChangeListener? Java Beans?
8. Dependency to OSGi in the API in EEFToolkitImpl.activate(ComponentContext)


##### OSGi & Declarative Services

1. OSGi Declarative services, why?
2. Why not use a couple of services and not use the XML-based dependency injection of OSGi? Our experience on some other project has highlighted various major maintainability issues.
3. Why not use a modern dependency injection framework based on annotations instead of XML like Google Guice, HK2, etc?

##### Meta-model

1. What is the role of the query meta-model?Why don't you use AQL and the Sirius interpretors? Why another mechanism?
2. Why does the name of Java appear in the meta-model? (JavaEditor, JavaView, JavaBody)
3. Why does ViewRepository have a RepositoryKind attribute? Why does this attribute start with an upper-case?
4. Why can we add views in a ViewsRepository and in a Category? It is confusing for new users.
5. What is the meaning of the field "explicit" in a View? Parental advisory explicit content?
6. Why does a view can contain another view?
7. Why is there at least four levels of recursions? Category, View, Container and ElementEditor?
8. Why not use a structure similar to Sirius? Viewpoint, Representation, Layer, Mapping, Style? With almost no recursions (Container mapping)
9. The field "representation" should not have this name. We should not use a name used in Sirius for a different concept. We should use the same naming conventions
10. What is a ViewElement compared to Sirius? A Binding, mapping, style?
11. The field nameAsLabel should not exist. It should use a string using a semantic expression (a regular String, service: or aql:).
12. We should reuse existing interpretors including those from Sirius.
13. Why does the concept ViewReference is in the meta-model? Isn't this a simple widget, like a hyperlink, which will change the view once used?
14. Why does the concept of CustomView exist?

##### Technical

1. Why overriding java.lang.Object#clone()? See EEFServiceProviderImpl.clone()
2. Can EEF work with Java services in the workspace?
3. Why does it need to load classes? org.eclipse.emf.eef.runtime.binding.settings.EEFBindingSettings.loadClass(String)
4. Multiple ways of doing the same thing should be avoided. See PropertiesEditingContextFactory.createPropertiesEditingContext(). Seven methods to do the same thing is too much. If this factory can be configured, we should use the builder pattern.
5. What does the concept of autowire mean? See ContextOptions.AUTOWIRING_OPTION. Is it similar to autowire in other dependency injection framework?
6. Why does PropertiesEditingComponent have a method getViewDescriptors() which returns a List<View> and a method getViews() which returns a List<Objec>?
7. bindingHandlerProvider.getBindingHandler(component.getEObject()).firePropertiesChanged(component, new PropertiesEditingEventImpl(evt.getSource(), evt.getPropertyName(), ((evt instanceof TypedPropertyChangedEvent)?((TypedPropertyChangedEvent)evt).getEventType():PropertiesEditingEvent.SET), evt.getOldValue(), evt.getNewValue()));
8. Why are there multiple notification mechanisms? PropertiesEditingListener vs PropertyChangeListener vs EEFNotification?
9. Why not use EMF notifications everywhere? Or one framework?
10. Why is there a separation for synchronous and asynchronous use cases? See PropertiesEditingEvent.delayedChanges()? Why not consider a synchronous use case as a simple asynchronous use case?
11. Why use a whole ResourceSet and not just the toolkit model in EEFToolkitImpl.getAllWidgetsFor(ResourceSet, EStructuralFeature)?
