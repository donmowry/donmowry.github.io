---
layout: post
title:  "Solid Engineering - D is for Dependency Inversion"
date:   2015-10-15  19:32:10 -0700
categories: programming solid
---

Despite sounding similar, this [SOLID]({% post_url 2015-04-07-solid-engineering %}) principle is Dependency Inversion, which is not Dependency Injection. Dependency Injection is simply the design of a class such that everything it needs is passed into it, which helps with such goals as single responsibility, unit testing, and removing hidden dependencies. [Dependency Inversion](http://www.oodesign.com/dependency-inversion-principle.html), however, is an inversion in the way that some might think about software development.  It is two rules:

> * High-level modules should not depend on low-level modules. Both should depend on abstractions.
> * Abstractions should not depend on details. Details should depend on abstractions.

You can read the example in the article above, but the key is that if there is a piece of functionality that couples two class together too tightly, abstract that out. You want to make sure that the interface between the two classes or systems are not talking to each other directly, but rather through a well-known interface. Similar thinking led to the USB spec, in that with the bus interface abstracted, different systems could plug into each other without having to be hard-wired together.

You want to make sure that things aren't coupled tightly where they should not be. This isn't to say that it should be abstractions all the way down, but especially between systems, there should be an abstraction to allow implementations to vary without changes needed on the other side. You want to stay nice and flexible.

Plus, you want to keep [Crusty](https://developer.apple.com/videos/play/wwdc2015/408/) happy.