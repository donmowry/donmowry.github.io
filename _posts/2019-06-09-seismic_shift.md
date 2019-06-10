---
layout: post
title:  "A Seismic Shift"
date:   2019-06-09  20:00:10 -0700
categories: [programming, apple] 
---

Did you feel that last week? If you are a developer on an Apple platform, did you feel that shift during WWDC?

Every so often, there is a shift so impactful that it's almost a physical feeling.  I remember the moment Swift was announced, and it was immediately clear that this was a huge change.  Maybe not right away, maybe not that year or the next, but these big changes reach out and are felt throughout the industry.  Java.  C++. .Net.  All of these had lasting impact.

Last week was another monumental shift.  SwiftUI, not even hinted at in rumor blogs except in the barest of ways, landed with an audible gasp and collective shock.  Not the iOS-on-Mac technology announced last year, here was a declarative, reactive UI framework, with deep tooling and support on all their platforms, the basis of Apple UI development for the next decade and beyond.

Through support in Swift 5.1, SwiftUI is a DSL for UI.  This hints at a greater power: to make this a domain-specific language, support for such a concept in Swift has been baked into the language. This means that DSLs can be created for other things. Can you image writing Swift code that actually expresses an HTML or CSS document? By creating SwiftUI, Apple has opened even more options, through Swift 5.1 and Combine.

Functional Reactive Programming has been a hot ticket in Apple development for a while now.  Since its seeds in C# a decade ago, it has grown and spread to other languages and platforms.  There are implementations for the Apple platforms, several in fact. But it has sat on top of the platform, lacking deep integration.  It has a different syntax and mindset that is not common to all developers, a barrier to hiring. Combine, the new Apple framework, changes that. It is an official framework that can bee integrated with platform technologies.  It is a platform framework, something that can rightfully be considered for hiring developers, like Core Data, GCD, or Core Animation.  It brings FRP into the fold.

Data flows one way in SwiftUI.  This is a well-known and well-regarded concept, but difficult to enforce, and tricky to always see the effects of.  But with FRP and SwiftUI, Data can be bound to views, and data flow enforced.

Bindings are native on macOS, but have never been available to iOS. Key Value Observing or Notifications could approximate bindings, but required glue code and have gotchas to be aware of.  Now bindings are available on macOS, iOS, watchOS, iPadOS, and tvOS.  This brings up another huge advantage of SwiftUI.

Cross-platform (where here, platform means Apple platforms) development remains an industry bugbear. Frameworks and tooling has lead mostly to lowest common denominator and alien UI. But here, there is no difference in platform tool. SwiftUI is _how_ you write UI code, from tvOS to watchOS and all computers in between.  And what is to stop Andorid, Windows, Linux, Web from joining in?  Not a conversion, not a lowest common denominator of support, but full platform support built right in?

Make no mistake: what Apple introduced last week is the way we will be programming UI.  Maybe not this year, maybe not next, but like with Swift itself, growing more and more each year, until it is massive and undeniable.  Giant.

When Apple dropped that giant news last week, did you feel that seismic shift?
