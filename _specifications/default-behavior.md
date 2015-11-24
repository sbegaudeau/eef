---
layout: specification
description: Default behavior
---

##### Default behavior of properties.ecore

By default if no properties view description is provided, EEF creates one properties view tab named *General*. 

One section per semantic element associated to the selection will be also created.

Each structural features should have a default widget.
The structural features are represented according to the order of the structural features in the EClass.

The following table defines how each structural features will be represented by default:


| Structural feature | Widget                   |
| ------------------ | ------------------------ |
| Single EAttribute  |                          |
|  EChar             | Text                     |
|  EString           | Text                     |
|  EInt              | Text                     |
|  EDouble           | Text                     |
|  EShort            | Text                     |
|  ELong             | Text                     |
|  EFloat            | Text                     |
|  EBigInteger       | Text                     |
|  EBigDecimal       | Text                     |
|  EByte             | Text                     |
|  EByteArray        | Text                     |
|  EBoolean          | Checkbox                 |
|  EEnum             | Radio or Combo           |
|  EDate             | DatePicker               |
|  EList             | MultipleValuesEditor     |
| Many EAttributes   | MultipleValuesEditor     |
| Single Containment | SingleReferenceViewer    |
| Many Containments  | MultipleReferencesViewer |
| Single EReference  | SingleReferenceViewer    |
| Many EReferences   | MultipleReferencesViewer |
| Default            | Text                     |

Non changeable attributes and derived features are represented as read-only by default.

###### Default widgets

* Text :
![Text]({{ site.baseurl }}specifications/images/default-behavior/Text.png "Text")
* Checkbox :
![Checkbox]({{ site.baseurl }}specifications/images/default-behavior/Checkbox.png "Checkbox")
* Radio :
![Radio]({{ site.baseurl }}specifications/images/default-behavior/Radio.png "Radio")
* DatePicker :
![DatePicker]({{ site.baseurl }}specifications/images/default-behavior/DatePicker.png "DatePicker")
* MultipleValuesEditor :
![MultipleValuesEditor]({{ site.baseurl }}specifications/images/default-behavior/MultipleValuesEditor.png "MultipleValuesEditor")
* SingleReferenceViewer :
![SingleReferenceViewer]({{ site.baseurl }}specifications/images/default-behavior/SingleReferenceViewer.png "SingleReferenceViewer")
* MultipleReferencesViewer :
![MultipleReferencesViewer]({{ site.baseurl }}specifications/images/default-behavior/MultipleReferencesViewer.png "MultipleReferencesViewer")

