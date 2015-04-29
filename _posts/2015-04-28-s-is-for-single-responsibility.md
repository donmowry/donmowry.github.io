---
layout: post
title:  "Solid Engineering - S is for Single Responsibility"
date:   2015-04-28
categories: programming solid
---

Suppose you were writing a server.  This server needed to serve a graph of data objects.  Furthermore, the data needed to be delivered in XML.  So, you write a nice interface and add a simple method to each data object:

{% highlight java %}
    public String toXML() {
        StringBuilder result = new StringBuilder();
        result.append( "<" ).append( "myObjectName" );
        for ( XMLizable child : children ) {
            result.append( child.toXML() );
        }
        result.append( "/>" ); 
        return result.toString();
    }
{% endhighlight %}

You can now call `toXML()` on the top level object and you are done.  Right?

You've just broken the [Single Responsibility Principle](http://en.wikipedia.org/wiki/Single_responsibility_principle).

It's easy to see immediate problems with this.  What if you need now need to deliver the data in JSON? Thrift? Plain HTML?  

What the Single Responsibility Principle states is that, essentially, a class should be responsible for only one thing.  This is typically stated as "[each software module should have one and only one reason to change](http://blog.8thlight.com/uncle-bob/2014/05/08/SingleReponsibilityPrinciple.html)".  Any reason that you can think of for a class to change means that it is *responsible* for that behavior.  In our example above, each data class is responsible for its own data, and also how to represent that data in XML. 

Sometimes the principle of encapsulation can make it seem like some functionality should belong to a class.  It seems reasonable that a class should not share its internals, thus should be the only one to know how to load itself from a database, or save itself back out.  But if we do that incorrectly, we've tightly coupled the class to a specific implementation of a database.  What if that database's interface changes?  Now we need to investigate every single data object to see if it needs to be updated.  What if we decide to change our storage implementation, from a relational database to a document?

View controllers can easily suffer from this.  It's a little vague what it means to *control* a group of views.  When a button is pressed, we need to know what to do next.  If this leads to a new page, perhaps there's business logic that needs to happen to finish off the current step in the application before moving on to the next.  A table might need its data, or see its cells set up a certain way.  A lot of times, those responsibilities can seep into the view controller itself.  But a lot of times, there is a better place for them.

It's interesting that the principle is phrased as having one "reason to change".  There is some implication in that specific wording that the best way to see a violation is when there is a need to change.  That is, our `toXML()` example is only problematic once we need to implement another data transport mechanism.  There are some responsibilities such as this that are clear from the very beginning.  Experience tells us that data transport should not be tightly coupled into data classes.  But perhaps there is a business logic module, and it is not clear that it has two or more reasons to change until it is time to make a change, and it becomes obvious that the method right next to the changed one really doesn't have anything to do with the overall purpose of the class.  Or the module grows over time to the point where two themes to the module start to emerge.

So why would we care about this in the first place?  What are the advantages to the Single Responsibility Principle?  One is that if we separate our responsibilities, we avoid tight <a href="http://en.wikipedia.org/wiki/Coupling_(computer_programming)">coupling</a>.  This makes it easier to change things without unintentionally affecting other things. This implicitly correlates to high <a href="http://en.wikipedia.org/wiki/Cohesion_(computer_science)">cohesion</a>, keeping related responsibility together, by keeping unrelated responsibility out.  Also, when classes have a single responsibility, they are simpler easier to follow, and easier to have complete test coverage.  This simplicity and testability lead directly to stability.

Why then would you not want to follow this principle right away?  For one thing, by separating concerns, you are necessarily introducing more classes into the codebase. This can make tracking down where said responsibility lives  more difficult, though once it is identified, it should be clear why it is separated.  And, we still need to ship. Designing a flexible, intuitive data transport transformation layer that will only ever serve JSON is not a good use of time or resources.   

As with all software principles, the pros and cons must be weighed and balanced.  In a language or framework that encourages [ActiveRecord](http://guides.rubyonrails.org/active_record_basics.html) type classes, for instance, data, validation, and persistence belong together.  Pure data objects might be better in others. 

Thinking carefully about responsibility when designing will improve your application.  If more than one reason to change emerge later, it's time for a refactoring.

Next in <a href="http://en.wikipedia.org/wiki/SOLID_(object-oriented_design)">SOLID</a>, the letter O: Open/Closed Principle. 

