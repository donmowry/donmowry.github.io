---
layout: post
title:  "Designing an Apple Watch App"
date:   2015-05-26
categories: apple watch programming design
---


Similar to [others](http://www.marco.org/2015/05/08/overcast-apple-watch-redesign), I found that when the concepts of a watch app meet actual hardware, the ideas give way to reality.

Given that this app hasn't shipped, I'm going to be speaking a bit in generalities.  The app has a hierarchy, a drill-down table view style, let's call it Level 1, Level 2, and Activity.  There are several kinds of Activities.  This should be sufficient to discuss my experience.

Last fall, after the Watch SDK shipped, I thought about what part of the app would benefit from the small, quick interactions that the watch would enable.  Right away, one of the Activities, one that had low interaction and value in short bursts, stood out as something that a user might want to do if they had a minute or less of time. This activity included an image downloaded from the Internet and a few pieces of text. For simplicity's sake, let's say the actions are thumbs up and thumbs down. 

After moving the data to [be accessible to both the app and the watch extension](https://developer.apple.com/library/prerelease/ios/documentation/General/Conceptual/ExtensibilityPG/ExtensionScenarios.html#//apple_ref/doc/uid/TP40014214-CH21-SW6), I created four branches: the first started with a list of Level 2 so that you had a sense of where you were.  The second started only with the activity, and used a long press to complete the action.  The third split the activity into 3 views to be swiped through, with the actions at the end.  The fourth had the actions on the first view with a little bit of data, and 2 more views.

So which one did I use?  It's probably apparent.

Immediately (and predictably), the first branch was out.  I had included this in order to prove that interacting with a long scrolling list was not ideal. And indeed, holding up your arm to scroll through a long list was instantly tiring. Introducing the extra step of deciding where to go slowed down the quick and direct access I was going for. *Lesson one: prioritize quick interactions.* 

A quick side note: I had feared that it would take a long time to send the image over to the watch. Surprisingly, this was nearly instantaneous. I had queued up some ideas about how to make this faster, but it turned out it was not needed right away, and I could move that optimization to a time when I had longer access to the hardware. 

The second branch started in a (essentially) random activity. You could scroll up and down to see the image and the text. Then, using a force touch, you would see the thumbs up / thumbs down options. This worked fine — and was clearly wrong. Bringing up the menu items included the taptic engine buzzing your wrist. You knew you were using a force touch. For what was to be many, quick interactions, this would have very soon become excessively annoying. As clearly demonstrated by the built in apps, the force touch was not a common interaction, but a way to change settings and the like. *Lesson two: use force touch for menu options only.*

For the third branch, I split the activity over three screens. The intention here was to make sure the user did not need to scroll, but instead would swipe through one screen of information at a time. This was a terrible idea. The Watch has a digital crown for the explicit purpose of scrolling. Swiping is more work, and should be used for things screens that are very different, like Glances, or different options like watch faces. Strongly related content like the activity that I was working on belongs together on one screen. *Lesson three: keep related content together.*

However, this was terrible for a different reason as well. I had the data loading very fast, and had the next activity ready to go. To display it, I reloaded the view controllers of the pages. But when I did this, there was a very noticeable lag. What I had feared for the images was now true for the data. This was similar to Today Extensions that might show old data for a moment. *Lesson four: keep view reuse to a minimum.*

I had come with four branches with varying differences, and none of them would be usable as they were. At this point, the interaction designer I was with and I had a few more hours with the hardware. So, I chose the only clear path.

Tear it all down.

Since I had written the app to completely separate the business logic and data fetching from the UI, it was a simple matter to swap out the front end.  Through experimentation, we'd learned that our app was best suited for a single screen, with normal text buttons for input.  To refresh the data quickly, I used a [WKInterfaceTable](https://developer.apple.com/library/ios/documentation/WatchKit/Reference/WKInterfaceTable_class/).  When you select an action, I can swap out rows quickly.  While my designer partner got to work on two animation sequences for success and failure, I reorganized the interface: a header, a table, and a group with two actions.  Our refreshed app felt intuitive, worked quickly, and had fun animation.  We left feeling buoyed by our accomplishment.

None of these lessons are surprising, and all are clear with hindsight.  But I think they exemplify the final lesson, which is also unsurprising, but needed to be done within the context of this specific app to see what worked best: 

*Experiment.*

