---
layout: post
title:  "Edutudu Part III: Vapor API"
date:   2019-11-06 23:07:10 -0600
categories: [programming, ios, edutudu]
---

In the [last post]({% post_url 2019-09-25-edutudu-part-ii-vapor-data %}) I created a simple model of data and compiled the app, but I commented out a portion of the templated code to do so. This portion dealt with persistence of the data and routing the data through a controller. My original intent was to hook into a MySQL database, but I think for the purposes of this exercise, I am going to keep the SQLite database. The principle is the same, and I may return to enhance the persistence later.  For now, though, I'm going to continue by fleshing out the web API.

From a cursory reading, the routing and controllers in Vapor seem similar in concept to Rails. Rails has [Active Record](https://guides.rubyonrails.org/active_record_basics.html) to save, load, and query data, while Vapor has Fluent.  Fluent doesn't seem to abstract the database driver away completely as we have to use specific protocols for specific drivers.  So, for instance, to persist and instance of a class in SQLite, the model must declare conformance to `SQLiteModel`.  I'd rather have a backing store-agnostic interface, but that's not what Fluent provides.

I had created the `Todo` struct with a `String` id property, thinking I'd use a UUID and convert it to a string. However, the Fluent SQLite package provides a `SQLiteUUIDModel` protocol that expects a UUID id field.  This is great, and I'll change the struct to match.

According to [the documentation](https://docs.vapor.codes/3.0/fluent/models/), "[b]oth structs and classes can conform to Model, however you must pay special attention to Fluent's return types if you use a struct."  This got me to thinking: I want a common definition for the data, but perhaps the individual apps should decide the best way to provide it? Should my common `struct`s be `protocol`s instead?

In designing an architecture for an app at work, I did exactly that. I created a library of `protocol`s to decouple the interface from the implementation and allow a plug-in type of component-based architecture. However, here, I believe that sticking to a `struct` is better.  A big reason is the `Codable` interface. It is simple to translate from JSON and back. This being an educational side project, I don't need additional complexity, for the same reasons I'm going with SQLite.

In a real application, I would spend more time designing this layer and do things differently.  One way might be to use protocols, and have a field that held the version of the data structure. This would allow encoding and decoding by switching on that version. Or perhaps the encoders and decoders are provided by the library, and the JSON only goes through there.  Designing for migration is essential in a production application.  For this project, not so much.

Thus, my `Todo.swift` becomes: 
```swift
import Foundation

public struct Todo: Codable {
    public var id: UUID?
    public let title: String
    public let createdOn: Date
    public let updatedOn: Date
    public let status: TodoStatus
    public let priority: Priority
    public let details: String?
    public let dueDate: Date?
}
```
and my `Todo+Vapor.swift` becomes:

```swift
import Vapor
import FluentSQLite

/// Allows `Todo` to be encoded to and decoded from HTTP messages.
extension Todo: Content { }

/// A single entry of a Todo list.
extension Todo: SQLiteUUIDModel {
}

/// Allows `Todo` to be used as a dynamic migration.
extension Todo: Migration { }

/// Allows `Todo` to be used as a dynamic parameter in route definitions.
extension Todo: Parameter { }
```

I'm going to create a [RESTful API](https://restfulapi.net).  Like Rails and other web app frameworks, Vapor makes this very simple. In fact, last time I commented out a bunch of generated code that supported the CRUD operations on `Todo`s in `TodoController.swift`.  I can uncomment that to get started, along with support in `routes.swift`.

At this point I run the app to see how things are going. I'm hit with a somewhat cryptic error:

`⚠️ DecodingError: Cannot initialize TodoStatus from invalid String value 1`

Huh?

Fortunately, [Google provides](https://github.com/vapor/vapor/issues/1736). The enum `TodoStatus` needs to conform to [ReflectionDecodable](https://api.vapor.codes/core/latest/Core/Protocols/ReflectionDecodable.html).  Of course in a certain way, this makes sense. An ORM needs to use reflection to inspect and understand objects so that it can create storage and map the object to the backing data store.  In another way, though, I'm still confused: the documentation explicitly states that there are types that are already made to conform to ReflectionDecodable, and that one is `String`.  And yet an enum of Strings needs explicit conformance. Well, if it must be, it must be:

```swift
import Vapor

extension TodoStatus: ReflectionDecodable {
    public static func reflectDecoded() throws -> (TodoStatus, TodoStatus) {
        return (.open, .complete)
    }
}
```

Another change that I've made is to store the SQLite file rather than keep it in-memory.  This will allow us to keep the data between application runs.  It's a simple change in `configure.swift` to provide a path to the `SQLiteDatabase` initializer. 

`let sqlite = try SQLiteDatabase(storage: .file(path: "edutudu.sqlite"))`

Running again, and going to [http://localhost:8080/todos](http://localhost:8080/todos) shows us a list of all none of our Todos. Let's try to POST one to see it show up in our list. 

There's no basic form generated in our scaffolding.  Vapor uses its own templating language, [Leaf](https://docs.vapor.codes/3.0/leaf/getting-started/) for this, but I'm going to defer that for now.  We'll just POST using cURL. 

Again, I'll mention a difference between this exploratory exercise and a real application.  This app is wide open, and in the real world we'd have some protection around it. Vapor provides [Auth](https://docs.vapor.codes/3.0/auth/getting-started/) for user authentication, which would be a must.  

To create a Todo, we can just POST JSON using cURL to our local address.  As I type it out, I'm wondering why I added so many fields 🤓. Also, I could handle different date formats, but ISO 8601 is fine by me. 

```bash
curl -H "Content-Type: application/json" -X POST -d '{"id": "cdefa388-581f-4dec-b1ea-6ad3019d6271", "title": "My first ToDo, by Fisher Price", "createdOn": "2019-11-07T05:50:39Z", "updatedOn": "2019-11-07T05:50:39Z", "status": "open", "priority": "high"}' http://localhost:8080/todos
```

And we get a response! 
```bash
{"status":"open","id":"CDEFA388-581F-4DEC-B1EA-6AD3019D6271","title":"My first ToDo, by Fisher Price","createdOn":"2019-11-07T05:50:39Z","priority":"high","updatedOn":"2019-11-07T05:50:39Z"}
```

Going back to [http://localhost:8080/todos], we still see no Todos, though. Opening the database and querying, we see nothing.  The reason is that I've supplied an `id`, which of course is not necessary as the ORM will be handling the creation of that on insertion.  It's strange that we don't get an error, but instead get the JSON mirrored back to us.  Anyway, let's try again:
```bash
curl -H "Content-Type: application/json" -X POST -d '{"title": "My first ToDo, by Fisher Price", "createdOn": "2019-11-07T05:50:39Z", "updatedOn": "2019-11-07T05:50:39Z", "status": "open", "priority": "high"}' http://localhost:8080/todos
```

The response:
```bash
{"status":"open","id":"0A927E2B-C003-4FEB-8884-B459213B8623","title":"My first ToDo, by Fisher Price","createdOn":"2019-11-07T05:50:39Z","priority":"high","updatedOn":"2019-11-07T05:50:39Z"}
```

And now refreshing the page we see all that glorious plain HTML:

```
[{"status":"open","id":"0A927E2B-C003-4FEB-8884-B459213B8623","title":"My first ToDo, by Fisher Price","createdOn":"2019-11-07T05:50:39Z","priority":"high","updatedOn":"2019-11-07T05:50:39Z"}]
```

That's all for now.  Next time we'll look at pulling some code into a Swift Package.

[Code for this post](https://github.com/donmowry/edutudu/tree/vapor-iii)

