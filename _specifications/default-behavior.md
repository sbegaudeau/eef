---
layout: specification
description: Default behavior
---

##### Default behavior of properties.ecore

By default if no properties view description is provided, Sirius creates one properties view tab named *General*. 

One section per semantic element associated to the selection will be also created.

Each structural features should have a default widget.
The structural features are represented according to the order of the structural features in the EClass.

The following table defines how each structural features will be represented by default:


| Structural feature | Widget               |
| ------------------ | -------------------- |
| Single EAttribute  |                      |
|  EChar             | Text                 |
|  EString           | Text / TextArea      |
|  EInt              | Text                 |
|  EDouble           | Text                 |
|  EShort            | Text                 |
|  ELong             | Text                 |
|  EFloat            | Text                 |
|  EBigInteger       | Text                 |
|  EBigDecimal       | Text                 |
|  EByte             | Text                 |
|  EByteArray        | Text                 |
|  EBoolean          | Checkbox             |
|  EEnum             | Radio                |
|  EDate             | DatePicker           |
|  EList             | MultiValueEditor     |
| Many EAttributes   | MultiValueEditor     |
| Single Containment | LinkEReferenceViewer |
| Many Containments  | Table                |
| Single EReference  | Combo                |
| Many EReferences   | Table                |
| Default            | Text                 |