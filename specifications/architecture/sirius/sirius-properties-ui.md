---
layout: specification
description: Sirius properties UI
---

This section explains the role of the `org.eclipse.sirius.properties.ui` plugin.

#### EEF Tab Descriptor Provider

This plugin contributes new tab descriptors thanks to the `EEF Tab Descriptor Provider` extension point.

The `SiriusTabDescriptorProvider` class implements the `IEEFTabDescriptorProvider` to return the list of computed tab descriptors.

#### View Description Converter