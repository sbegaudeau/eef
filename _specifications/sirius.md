--
layout: specification
description: Styles
---
We need to be able to provide a clear and simple integration of EEF for a Sirius specifier.

##### User stories

###### US-1 As a Sirius specifier, Zoe wants to get beautiful properties view without specifying anything

Zoe needs to get beautiful properties view by default. She do not want to specify anything special for the properties view when she develops her Sirius specification.
The EEF runtime provides dynamically by default enhanced properties view depending on the type of each meta-model elements to represent. 
* EString attribute is represented with a label set to the attribute name and a text field to hold the value.
* EBoolean attribute is represented with a label set to the attribute name and a checkbox.
* Single EReference is represented with a combo.
* Multiple EReference is represented with a table.

###### US-2 As a Sirius specifier, Zoe wants to customize the default rendered properties view

Zoe needs to customize some fields of the default properties view rendered by the EEF runtime dynamically. The EEF tooling provides a tool to create a default properties view specification based on the rules used by the EEF runtime to render the default properties view. If no EEF specification is defined, the EEF runtime provides a default properties view else the EEF specification is used.  

##### Functional Specification

##### TODO
How to integrate EEF tooling with the Sirius existing tooling? Is it two different specification? Does the EEF specification is integrated in the odesign as a new dialect ?