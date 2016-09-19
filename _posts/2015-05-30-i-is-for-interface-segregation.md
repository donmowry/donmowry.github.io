---
layout: post
title:  "Solid Engineering - I is for Interface Segregation"
date:   2015-05-30
categories: programming solid
---

Keeping the [hot side hot and the cold side cold](http://www.seriouseats.com/2011/02/blast-from-the-past-the-mcdlt-mcdonalds-1980s-jason-alexander.html) may have been a marketing or ecological failure, but keeping dissimilar responsibilities apart is just good object-oriented programming, and is today's [SOLID]({% post_url 2015-04-07-solid-engineering %}) principle: the Interface Separation principle.

At its core, this principle is about introducing high cohesion to our objects. This means that an interface should consist of methods and properties that are tightly replaced to each other. If a class suffers from low cohesion, it will be larger, more unwieldy, and more difficult to change and maintain. Clients of a class should respectfully be presented with a small, focused interface. 

On iOS we have a common example of this principle. When you use a Table View Controller, there are two protocols you implement as well: a UITableViewDataSource and a UITableViewDelegate. The table view uses both of these in its operation. Yet, functions related to how a table view gets its data are not related to communication from the table view to its delegate. Thus, it would not make sense to combine these interfaces. In your own code, you could have one class that implements both protocols, but there is no reason that one object must implement both. 

When designing class interfaces, you are creating an API, even if the only client will be your internal code. ISP is about treating the clients of this API with respect. By decoupling disparate methods, clients are presented with a slimmer, clearer surface because the implementation is not forced to implement methods it does not need. That's just common courtesy right there. 
