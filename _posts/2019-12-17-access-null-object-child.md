---
layout: post
title: A safe way to access a null object's children
date: 2019-11-28 00:00:00
---

Have you ever encountered this error?

```
Uncaught TypeError: Cannot read property 'name' of null
    at <anonymous>:5:31
```

It's probably because you tried to access the child object of a ``null`` object inline. Let's take a look at some different scenarios.

Here the nested object is complete. All child objects have a value, and as we expect using ``console.log`` does not throw an error.

```js
var salesOrder = {
    author: {
        name: 'John',
        lastName: 'Doe'
    }
}

console.log(salesOrder.author.name) // John
```

In this scenario, the first-level object ``author`` is set to null.  When you try to access the ``name`` object, you'll get an error.
```js
var salesOrder = {
    author: null
}

console.log(salesOrder.author.name)
```
And as you may guess it's the ever infamous ***cannot read property 'value' of null***
```
Uncaught TypeError: Cannot read property 'name' of null
    at <anonymous>:5:31
```

The safest way to go about this issue, is to access the object through **[short-circuit evailuation](https://en.wikipedia.org/wiki/Short-circuit_evaluation)**. It's a feature in most languages that simplifies an inline ``if-else`` statement and provides a safe way of unwrapping objects.

Some languages implement it differently, but they still provide the same output.

**Javascript**

```js
var salesOrder = {
    author: null
}

console.log(salesOrder && salesOrder.author && salesOrder.author.name) // null
```

**C#**
```cs
class Author {
    public string name { get; set; }
    public string lastName { get; set; }
}

class SalesOrder {
    public Author Author { get; set; }
}

var salesOrder = new SalesOrder {
    Author = null
};

Console.WriteLine(salesOrder?.author?.name) // null, but does not throw an error
```

*Not so funny thought of the day:*
> Have you ever thought about how most Software questions you ask a search engine may trigger some kind of switch in the FBI? 'How to fork multiple child', 'Kill parent', 'Destroy object'. 
