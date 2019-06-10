---
layout: post
title:  "Solid Engineering - L is for Liskov Substitution"
date:   2015-10-01  19:32:10 -0700
categories: programming solid
---

Halfway through [SOLID]({% post_url 2015-04-07-solid-engineering %}) we find the letter L.  This stands for the Liskov Substitution Principle.  Since it's self-evident what this means, we can move on.

Um.

[Bertrand Meyer](http://www.indiebound.org/book/9780136291558) also discussed this principle, but Barbara Liskov introduced and formalized it in a stricter way, thus this principle bears her name.  This principle lays out a strong behavioral relationship of subtypes such that a class may be replaced with a subtype of a class without altering the correctness of a system.  

This has [design by contract](http://en.wikipedia.org/wiki/Design_by_contract) implications.  Subtypes may not have stricter preconditions or looser postconditions than their parent type. So what does this mean?

A common example given is a Square class.  A Square is a type of Rectangle, so it makes sense that Square inherits from Rectangle. However, a Square has an additional, stricter precondition: the height and width must be identical.  Therefore, if we have setter methods for one or the other, it would need to mutate both dimensions.  If a Square is used where a Rectangle is expected, this stricter precondition may cause unexpected consequences, as code that mutates a Rectangle's height is not expected to affect its width.

This is clear, though probably less applicable explanation of a violation of the principle. This also allows a finer point about the LSP: the stricter condition being introduced is that _when the height or width is changed, the other is changed as well_. However, here there is a simple correction to this: remove the _change_ condition. Make the object immutable.  The calling code then can have no expectation that changing one dimension does not change the other, simply because it can have no expectation of change at all.

This shows that the principle is about contracts, expectations, and behavior.  It is the expected behavior that cannot change from type to subtype. 

This is a fairly subtle point, and one that is probably rarely violated, but when it is, it can cause unexpected side effects and crashes.

Next up: I.

