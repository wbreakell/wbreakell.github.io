---
layout: post
title: "Get and Set"
---

Originally introduced in ECMAScript 5.1, the `get` and `set` syntax never saw much widespread use in the JavaScript ecosystem. Only recently, with the introduction of the `class` syntax in ECMAScript 2015, have the `get` and `set` keywords seen a renewed interest among developers who need greater control over the properties in their objects.

The `get` and `set` syntax provides JavaScript developers with many of the benefits of [encapsulation][encapsulation], found in other traditional object-oriented languages. It can limit direct access to object properties and give the developer control over the process of retrieving and setting object property values.

Both the `get` and `set` keywords will bind an object property to a function. With `get`, this function is then called whenever there is an attempt to retrieve the value of that property. With `set`, the function will be called whenever there is an attempt to set the value of that object property. This technique allows us, as developers, to read and write the properties of an object through functions, executing any code we wish when someone tries to get or set the value of an object property.

{% highlight javascript %}
class Person {

  constructor(name) {
    this._name = name;
  }

}

let p1 = new Person('Walter');

p1._name // "Walter"
p1.name // undefined
{% endhighlight %}

In the example above, I’m declaring a class called `Person`. When an object is created based on this class, the constructor expects a value for the name of the person. This value is then assigned to a backing field called `_name`. However when I use the `p1` object created from this class, I’d like to access the name by just calling `p1.name`. Let’s accomplish this by creating a getter function for the name property.

{% highlight javascript %}
class Person {

  constructor(name) {
    this._name = name;
  }

  get name() {
    return this._name;
  }

}

let p1 = new Person('Walter');

p1.name // "Walter"
{% endhighlight %}

Now, by using the `get` keyword I have created a getter method that will return the value of `this._name` whenever I try to access the `name` property. This allows me as a developer to control the process of retrieving this property. I don’t necessarily need to just return the value of `this._name`. I can perform any actions I’d like within the body of the getter method. For example, I may want to return an all uppercase version of the name.

{% highlight javascript %}
get name() {
  return this._name.toUpperCase();
}
{% endhighlight %}

If I’d also like to set the value of the `name` property, I can create a setter method using the `set` keyword.

{% highlight javascript %}
class Person {

  constructor(name) {
    this._name = name;
  }

  get name() {
    return this._name;
  }

  set name(newValue) {
    this._name = newValue;
  }

}

let p1 = new Person('Walter');

p1.name // "Walter"
p1.name = 'James';
p1.name // "James"
{% endhighlight %}

This setter method allows me to change the value of the name property by simply calling `p1.name = 'James'`. Behind the scenes, the method takes my new value as an argument and assigns that new value to our backing field, the `_name` property. Just like the getter method, this new setter method gives me the flexibility to execute any code I wish within the body of the method. I may want to log every time the name property is changed, or I may perform some validation of the new value before actually writing to the name property. In this case, I’ll ensure that any new value assigned to the name property must be less than ten characters in length.

{% highlight javascript %}
set name(newValue) {
  if (newValue.length < 10) {
    this._name = newValue;
  }
}
{% endhighlight %}

While the `get` and `set` syntax gives us a convenient way to provide some encapsulation for our code, it does not completely protect our object properties. Anything I place in a JavaScript object is publicly available, and there is nothing stopping someone from directly interacting with our `_name` property. Still, the `get` and `set` keywords provide us, as developers, with an easy way of controlling how we read and write properties on our objects.

[encapsulation]: https://en.wikipedia.org/wiki/Encapsulation_(computer_programming)
