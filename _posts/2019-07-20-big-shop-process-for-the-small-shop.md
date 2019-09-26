---
layout: post
title:  "Big Shop Process for the Small Shop"
date:   2019-07-20 20:00:10 -0700
categories: programming process 
---

While there's a lot of discussion of programming tips and tricks, of the latest APIs and libraries, I wanted to discuss a little of the practice of software engineering, of the scaffolding around the procedure of writing code that helps to enable that code to go out into the world and help the people that use it.  There are number of processes in place in bigger development shops which are also of benefit to the small, or one-person, firm. Some of these are basic, others may seem overkill for one person.  You don't have to do them all, or do them as I lay out here.  However, done well, they help the generation and smooth the delivery of quality code.

Like a great many developers, I work full time but also do some programming at home. I do this to keep sharp, explore new techniques, use APIs that I may not get a chance to at work, and just to have something of my own to polish and refine. I have other hobbies, but programming is definitely one of them.  This article comes from my experience in applying the procedures I use in my day-to-day job to my own projects at home. I believe it would equally apply to a single-proprietor shop, a freelance contractor, or a small group.

Let's start with the basics. 

## Version Control
I'm just going to assume that you're doing this. Version control, whether using Git, Mecurial, or even SVN or CVS, is a fundamental tool of the engineer. The main reason that I mention it is that I'll be using Git throughout this discussion. You may be using another tool, and the topics still apply though the details may change. 

Make sure that code is committed regularly, with good comments. Don't be the one that future-you is cursing out later because you can't find a commit that caused a crash.

### Off-site 

Even for the small or one-person firm, off-site storage of code is important.  Like backing up your valued data on your computer, backing up your code off-site increases the safety and reliability of your storage. This doesn't mean Dropbox (though that's a good second line of defense). We're talking about version control. There are many providers, such as [GitHub](https://github.com), [GitLab](https://gitlab.com), and [Beanstalk](https://beanstalkapp.com), that offer free or low price hosting for small projects.  You can see a larger list at [Wikipedia](https://en.wikipedia.org/wiki/Comparison_of_source-code-hosting_facilities).  There's no reason not to do this. I'm using [Bitbucket](https://bitbucket.org/product/), for example, on a side project.

## Git Flow

In his [seminal post](https://nvie.com/posts/a-successful-git-branching-model/), [Vincent Driessen](https://nvie.com) laid out a model for git branching that came to be known as Git Flow.  It's since been codified into a tool called git-flow that's available on many platforms, such as on macOS through Homebrew.  

However even if you don't install the tooling, the concepts are sound and can be used without it.  At its simplest core, the key points are that `develop` is the branch representing the current in-progress code, `master` is only released code, and all work happens on branches merged to `develop`.  If nothing else, this is a minimum version control model to follow.

What is most important is that there is a process, and that it is followed.  It's very simple for a single developer to skip process since there is no one else to check that it is followed. Discipline is part of a successful single-person shop.

Along with the previous two points, this forms a  process of committing often, with good comments, to a feature branch on an off-site server.  You will be happy with yourself that you do.

## Issue Tracking

If you are or have ever been a professional developer, you are familiar with issue tracking software.  You may have even written some yourself.  

Creating issues for yourself may seem like overkill.  You know what you need to do and can do it.  However, what issue tracking represents is a bit of Project Management.  If you create issues for everything that you plan to do, you can more easily track and complete things. You can add future enhancements.  You can track bugs that your users may tell you about or that you find, and determine how severe they are. Issue tracking is most basic way to manage your overall project.

This can be as simple as a spreadsheet. But here, again, there are free or low cost tools. Bitbucket and Github have built-in issue tracking. [Trello](https://trello.com) is a [Kanban](https://en.wikipedia.org/wiki/Kanban_(development)) board that is free at small scale. The important part is to get things written down so they can be ordered and you'll know what to do next. 

## Pull Request

When you push up a branch, you can make a pull request, asking a maintainer to merge into the main branch.  Using a Git Flow model, this would be a merge into the `develop` branch. Github and Bitbucket and others provide helpful tools for seeing the diff of the code being proposed. At this point, it can be read, examined, and reviewed before being accepted.  The reviewer can make comments, the submitter update the branch, etc. 

So, what would be the point if there is only one person on the project? One person that has written the code, one person reviewing, one person merging? It's helpful for a few reasons. One, it enforces the branching model above. Code must be branched before changing in order to use a PR process. It will help you stick to making small changes per PR so that they are readable, a good practice that goes hand-in-hand with the small commits with good comment mentioned above. Another is that it gives you a chance to see your changes in isolation, all in one place.  This will help you spot such things as errors, stray changes, and missing documentation. Finally, if you bring on a collaborator later, it will be good to have the process in place.

Tooling makes this so simple it's practically as easy to just do it as not.

## Continuous Integration

When you merge code, you want to make sure it works.  You want to make sure it builds from source on a different machine and you want to make sure all the tests pass.  If you don't, you could be surprised that you've accidentally set up your project such that it depends on unidentified environment requirements, or as I once did, on a custom framework that could not be rebuilt. [Continuous Integration](https://en.wikipedia.org/wiki/Continuous_integration) can ensure this.  

For the small shop, frequent integration, periodically building and running the code and tests, can be sufficient. But there are tools available, for instance [Travis CI](https://travis-ci.org) has an option to build any open source project on Github, or built from source for a local server. [Jenkins](https://jenkins.io) and other tools can be built from source as well. These can give piece of mind and remove the need for manual integration.

## Continuous Delivery

Just like the code can be integrated automatically, that built code can be distributed automatically.  You can use your CI server to automatically send binaries to services like HockeyApp or TestFlight. This is especially important when beta testing with a larger pool of users. 

Like CI, CD may be overkill for a one person project.  It's simple to build and upload manually.  But even if this CI/CD is done with simple local scripts or tools such as [Fastlane](https://fastlane.tools), automatic CI/CD saves time and thought. Scripted processes are automatic and repeatable. 

## Tag & Release

This another place where where following the git-flow model helps. When you are ready, create a release branch. If you have a CI/CD process set up, at this point you can have the release branch automatically pushed up through your beta channel, whether that's TestFlight, AppCenter, Google Play, etc. 

When you release live, merge the release branch to `master`, delete the branch and tag master with the release version. Keeping consistent with this process is simple and will ensure that your `master` branch always reflects what people who use your app are seeing. 

## Beta Testing

When you have a release branch as an app in your beta channel, it's time to make sure that you know the bugs that you are shipping with. Note that you'll likely be shipping with bugs. As software gets more complex, there are more ways for small errors or incorrect interactions between components to enter. There are great ways to minimize the number of bugs, which are out of the scope of this discussion, but you are likely to have bugs. It's important to know what these may be and to ensure that you don't ship with *major* bugs, show-stoppers like user data loss or corruption, UI dead ends, deadlocks, memory mismanagement, etc. 

To help determine that the app is consistent enough for your users, and to shake out issues, beta testing is an important step. Getting enough outside eyes on your app, eyes that are sharp and trusted, will reveal problems that you may subconsciously gloss over since you are so familiar with your own product. Just as a copy editor can spot tpyos and grammatical errors that the author's mind skips over, so too do beta testers spot the small things. Just as an editor may reveal structural problems that the author is too close to see, so too do beta testers give a report of how a clean mind perceives your app. Even subjective things like the duration or direction of an animation are important to get a reading on. If your testers are saying a fancy animation is actually getting in the way of accomplishing a task, then it doesn't matter how enamored you are with it. That's an issue. 

For each issue raised, you must assess: does this need to be fixed before shipping? Can it wait until a subsequent release, a fast-follow release or next major version? The severity of the issue and complication of a fix sometimes makes this clear, but other times it's a judgement call. 

There are services to help with this, but for those projects with limited resources, recruiting people you know or those from social networks or forums may be sufficient. It's more helpful if they are a part of the target audience. 

Beta testers can help with the correctness, perception, and usability of your app. They can tell you exactly what they were doing then they encountered something. Getting a report of a problem from a beta tester is far better than getting one from the field. 

## Wrapping up

This list may be more aspirational for me than actual. I certainly miss steps or cut corners at times.  But there is a right way to do things, and just like I would try to never knowingly commit a bad hack, I know what the right process is and strive to follow it. Hints of process from bigger development shops can help the one-person side project make the best use of the limited time there is.


