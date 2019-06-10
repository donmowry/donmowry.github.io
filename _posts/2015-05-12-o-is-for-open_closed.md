---
layout: post
title:  "Solid Engineering - O is for Open/Closed"
date:   2015-05-12  19:32:10 -0700
categories: programming solid
---

Our next stop on the SOLID tour is the Open/Closed principle. [Bertrand Meyer](http://en.wikipedia.org/wiki/Bertrand_Meyer) coined this phrase, saying that classes should be "[open for extension, but closed for modification](http://www.indiebound.org/book/9780136291558)".  What this means is that once defined, classes should not be edited in order to add features.  

Let's say we have a mortgage application.  We have a class representing a mortgage that will calculate the current payment required through one method, `calc_payment`.

THRILLING!

Let's take a look at our method:

{% highlight python %}
	def calc_payment(self):
	    num_payment = self.years * 12
	    monthly_payment = self.principal * \
	        (self.interest_rate / (1-math.pow((1+self.interest_rate), (-num_payment))))
	    return monthly_payment
{% endhighlight %}

This is great.  So simple. A = P(1 + r^(-t)).  Done.

Wait.  What about an [ARM](http://www-old.me.gatech.edu/jonathan.colton/me4210/c1adjustloan.pdf)?

Hmm, so, we can open this class, add an `if` statement, and that's fine, right?

What if our state becomes California and suddenly Interest-Only loans are popular?

The problem here is that adding a feature of a new loan type requires opening and editing this class.  This makes the code's complexity grow over time, and tightly couples the calculation of the loan to the mortgage class.  This class is becoming fragile and difficult to change.

There are a few ways to solve this problem.  Most descriptions of Open/Closed principle discuss solving this through an abstract base class.  `Mortgage`, our base class, could have a `calc_monthly()` function.  Each concrete base class, such as a `FixedRateMortgage` or `AdjustableRateMortgage` overrides this function and provides the proper algorithm.  However, we should also prefer composition over inheritance.  In this way, it is easy to see a property of the `Mortgage` class that conforms to the interface `MortgageType`, where the calculation lives.  Either way, the `Mortgage` class and all of the subclasses do not need to be edited in order to add a new feature to our application.  Thus, they are closed for modification.  

Note that the definition specifically does not preclude bug fixes.  Having bugs in your program does not (necessarily) mean you are violating the Open/Closed principle just because you will have to open the class to fix them.  It just means you have bugs and need to get to work, and also probably need a couple more unit tests.

Next letter: L.   