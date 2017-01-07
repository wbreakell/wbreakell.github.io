---
layout: post
title: "With Statement"
---

The purpose of the `with` statement in JavaScript is to provide a shorthand way of accessing multiple object properties. It allows you to essentially treat the properties of a given object as local variables within the statement. It's intended to provide a nice convenience for developers and save some repetitive typing.

For example, if I have an object with multiple properties and want to perform a number of operations or assignments with those properties, it can become a bit of a hassle to always need to specify the name of the object before accessing the property I want.

{% highlight javascript %}
var foo = {
  a: 5,
  b: 3,
  c: 1
};

foo.a = foo.b + foo.c;
foo.c = foo.a - foo.b;
{% endhighlight %}

The `with` statement gives me a slightly more convenient way of writing these same statements. It allows me to specify the object I'm trying to reference up front, and then simply access the properties of that object without having to identify the object those properties belong to each time.

{% highlight javascript %}
with (foo) {
  a = b + c;
  c = a - b;
}
{% endhighlight %}

In this case the `with` statement gives us a quicker way of writing these statements. While this might appear fine in this simple example, use of the `with` statement in your programs is highly discouraged for a number of reasons.

Let's say we now want to create a new property of the `foo` object using the `with` statement.

{% highlight javascript %}
with (foo) {
  a = b + c;
  c = a - b;
  d = 3;
}
{% endhighlight %}

The code above makes it appear as though we are creating a new property of the `foo` object called `d` and assigning it the value `3`. However, what we are actually doing is creating a new global variable called `d` with the value `3`.

The `with` statement is actually creating a new lexical scope in the same way that wrapping some statements in a function declaration creates a new lexical scope. The JavaScript compiler will look within the scope of the `with` block and be unable to find a reference to a variable called `d`. The compiler will then move up one level in scope, to the global scope, and search for a reference to a variable called d. It won't find any preexisting reference, and will therefore create a new global variable called `d` with the value `3`.

In this case, I didn't intend to create a brand new global variable, but because of the way lexical scoping behaves in JavaScript, this new global variable will be automatically created.

The reason this type of automatic global variable creation is occurring is because the `d` property does not already exist on the `foo` object. When I'm assigning a new value to the property `a` within the `with` statement, the `a` property already exists so the lexical scope lookup works as expected.

Another reason the `with` statement should be avoided is because of the performance implications it can create. Before actually executing your code, a JavaScript engine will typically try to statically analyze the statements you've written, looking for all the variable and function declarations in your code. This gives the engine some awareness of where identifiers are located and how these identifiers should be scoped.

The `with` statement creates an entirely new lexical scope at runtime. The JavaScript engine, therefore, has no way of knowing how the object you pass to the `with` statement might affect its initial scope assumptions. Any assumptions made about the way your program is scoped could be completely invalid. As a result, whenever the JavaScript compiler sees a `with` statement it assumes the worst case scenario and must disable many of its optimizations. Simply including the `with` statement in your program can have a major impact on how fast your JavaScript is able to run.

While the `with` statement is extremely discouraged, it is actually prohibited when running your code in [strict mode][mdn-strict].

[mdn-strict]: https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Strict_mode
