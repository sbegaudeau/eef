---
layout: userstory
title: CLA-1
description: As Claire, I need to have continuous integration
---
In order to have continuous integration of my project we need:

* Maven and Tycho-based build
* Junit-based unit tests running in the build
* Checkstyle-based static analysis
* Hudson-based build for the branch
* Hudson-based build for Gerrit
* Promotion on a nightly update site
* Aggregation in the Simultaneous Release

The EEF project could also be moved to /modeling/eef in order to have the liberty to choose what we can add in our Bugzilla. Gerrit would also be a must have on the project.

* [Move EEF to modeling/eef](https://bugs.eclipse.org/bugs/show_bug.cgi?id=473512)
* [Gerrit for EEF](https://bugs.eclipse.org/bugs/show_bug.cgi?id=473516)
