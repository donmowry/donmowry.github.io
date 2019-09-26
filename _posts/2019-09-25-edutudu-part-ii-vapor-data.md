---
layout: post
title:  "Edutudu Part II: Vapor Data"
date:   2019-09-25 18:27:10 -0600
categories: [programming, ios, edutudu]
---

I love data.  Data is the lifeblood of any application. Table views? Full of data.  Email? Nothing but data. What is Instagram the app but a shell to hold the data of pictures and posts? I love a great UI. Native platforms have the best ability to create smooth, interactive, natural experiences and fluid animations that can be just plain gorgeous. Developing these UIs is deeply satisfying. But that UI needs to be filled with data, or it's just a lifeless picture frame.

The To Do app needs some data definition.  The Todo class that was created by the Vapor template needs to be expanded on, and some relations build around it. We need to step back and think about the final app, what structures we need, and the properties of those structures.

I want to keep this application minimalist, not get overloaded with details at any one level, so that I can go full stack.  Once I've achieved that, I can go back in and start adding things, areas that I want to learn more about (in keeping with the purpose of the project) or just areas that are interesting. To that end, I'm going to keep the number and complexity of the objects simple.

At the very least, a To Do app is going to need something to represent the item to be done. It should also have a person that has things To Do, a User.  

Quick aside: I've grown to dislike the word "User" to describe a person using an application.  I'm fortunate in my day job that it is a teaching application, so we can talk about "Learners" in general, instead of Users.  However, even our data tables and objects use the term "User", so I'm not quite sure there's a better term.

A Todo will need a properties to hold the text of the item, completion status, a creation date, update date, and let's put in an optional due date and optional details for use in exercising a future UI.  Let's also give it a [priority](http://www.43folders.com/2009/04/28/priorities) to torture ourselves.

A User will have a name, creation date, update date, and will have a list of Todos. Both of these will need unique Ids to, well, uniquely identify them in the database.

Vapor applications model data objects using a protocol: [`Content`](https://docs.vapor.codes/3.0/getting-started/content/). This in turn conforms to three protocols: `Codable`, `RequestDecodable`, and `ResponseEncodable`.  The first is the Swift protocol we already know and love.  The second two allow Vapor to send and receive objects in HTTP requests and responses.  However, `Content` provides default implementations of the `RequestDecodable` and `ResponseEncodable` functions.  This means that the only requirement for a data object to be marked as `Contnet` is that it conforms to `Codable`. 

Since I eventually want to pull the data and any business logic into a Swift package, I'm going to begin with that in mind and create `struct`s in a group, and the Vapor conformance in another group:

![model creation](/assets/adding-todo-models.png)

I can reuse the existing `Todo.swift` file in the App > Models group.  This is a `class` by default, and I'm not sure I need the reference semantics here - could this be something that Vapor needs? I don't know, but I'll find out! Because I'm going to start with a `struct`.

```swift
import Foundation

public struct Todo: Codable {
    let id: String
    let title: String
    let createdOn: Date
    let updatedOn: Date
    let status: TodoStatus
    let priority: Priority
    let details: String?
    let dueDate: Date?
}
```
Then, I can rename the original `Todo.swift` to `Todo+Vapor.swift` and replace the contents with this:

```swift
import Vapor

/// Allows `Todo` to be encoded to and decoded from HTTP messages.
extension Todo: Content { }
```
At this point, I'm also going to comment out all of the database, controller, and route information having to do with Todos and TodoControllers.  By default, the template is using an in-memory SQLite store, and I'm going to address that in a future post.

I can run, it all compiles, and the home page displays "It works!"  Indeed it does, little vapor app.  It just doesn't do much yet.

[Code for this post](https://github.com/donmowry/edutudu/tree/vapor-ii)

