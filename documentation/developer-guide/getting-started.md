---
layout: specification
description: Getting started
---

The new EEF framework integrated with Sirius will be released with EEF 1.6.0 and Sirius 4.0.0.as experimental in the next Neon release.
Be aware that as the framework is still *under construction* the APIs are evolving a lot.

#### Development environment

Get the [Oomph configuration for Sirius](https://wiki.eclipse.org/Sirius/Contributor_Guide "Oomph config for Sirius"). 

#### Getting the sources

The Oomph configuration already clones the **Sirius** repository. The Sirius source code relative to the EEF integration is available on the Sirius git repository on the *master* branch in the [incubation](http://git.eclipse.org/c/sirius/org.eclipse.sirius.git/tree/incubation) folder. 

Then, you need to clone the **EEF** repository : `git clone https://git.eclipse.org/r/eef/org.eclipse.eef`. The EEF source code is available on the [EEF git repository](http://git.eclipse.org/c/eef/org.eclipse.eef.git/tree/ "EEF"). The code is on the *master* branch in the plugins prefixed by *org.eclipse.eef*.

Import the EEF sources from the git repository in your Eclipse.

Finally, open the `sirius_neon.target` file from the `org.eclipse.sirius.targets` project in the target editor and (once it is loaded) set it as the current target.

#### Contribute properties view description

Launch an Eclipse runtime. Open an existing *Viewpoint Specification project*. In the `.odesign` file contribute a new `View Extension Description`.

![ViewExtensionDescription]({{ site.baseurl }}documentation/developer-guide/images/ViewExtensionDescription.png)

##### Page

Then contribute a new `Page Description`.
Set the `Label Expression` to *Page* and the `Semantic Candidate Expression` to *var:self*.

##### Group

Define a new `Group Description` and set its `Label Expression` to *Group* and the `Semantic Candidate Expression` to *var:self*. Set the `Groups` reference in the previously created page to select the newly created group.

##### Container

Under the group create a new `Container Description`.

##### Widgets

Under the container create some widgets. For example you can create a `Text Description`. You need to set the `Label Expression` and the `Value Expression`.

![WidgetsDescription]({{ site.baseurl }}documentation/developer-guide/images/WidgetsDescription.png)

Have a look to the [widget specification documentation]({{ site.baseurl }}specifications/architecture/eef/widgets) for more details.
