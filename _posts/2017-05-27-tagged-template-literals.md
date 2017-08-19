---
layout: post
title: "Tagged Template Literals"
---

Starting in ECMAScript 2015, template literals have allowed developers to express and manipulate strings in powerful new ways. The template literal syntax provides convenient ways of representing multi-line strings and allows for embedded expressions. However, a more advanced form of the template literal syntax can enable even greater control.

Template literals can be associated with a tag, allowing them to be parsed by a function. These *tagged* template literals give developers full control over how the template is manipulated and eventually output. Let's look at how we might create a custom tagged template literal.

{% highlight javascript %}
let uppercase = (strings, ...values) => {
  let result = '';
  for (let i = 0; i < strings.length; i++) {
    result += strings[i];
    if (i < values.length) {
      result += values[i];
    }
  }
  return result.toUpperCase();
};

let a = 2;
let b = 5;
let output = uppercase`When I add ${a} and ${b}, the result is ${a + b}`;
console.log(output); // "WHEN I ADD 2 AND 5, THE RESULT IS 7"
{% endhighlight %}

In the example above, we have created a tag function called `uppercase`. This function is responsible for parsing any template literals that begin with the `uppercase` tag prefix.

The tag function above takes multiple arguments. The first argument is an array of the string values found in the template literal. In our case, this array of strings is `["When I add ", " and ", ", the result is ", ""]`. This array represents the pieces of literal text in the template. They are separated into an array with the embedded expressions removed. The remaining arguments are the values of the expressions being used inside the template. In the example above, we are using a [rest parameter][rest-parameter] to represent those expression values as an array. This array will be `[2, 5, 7]`.

Within the `uppercase` tag function, I can interpret these incoming parameters in any way I'd like. I can alter text, rearrange values, and ultimately return any type of manipulated string I would like. In fact, tag functions don't even need to return a string.

However, in our simple example, we just want to build the same string that the template literal would normally produce and then convert it into uppercase text. To achieve this, we use a `for` loop to combine the string values with our expression values and store this assembled string in the `result` variable. Finally, we call the `toUpperCase()` method on our result string and return that value from the function.

Although our `uppercase` tag function is extremely simple, it demonstrates how flexible and powerful tagged template literals can be. Tagged template literals could be used to perform safe HTML escaping for strings, preventing XSS attacks. You could use tagged template literals to apply translation and internationalization to your codebase. There are already a number of exciting open source projects, like [styled-components][styled-components] and [graphql-tag][graphql-tag], using tagged template literals in really unique ways. This syntax and functionality provides JavaScript developers with an incredibly versatile approach for transforming and modifying templates.

[rest-parameter]: https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions/rest_parameters
[styled-components]: https://github.com/styled-components/styled-components
[graphql-tag]: https://github.com/apollographql/graphql-tag
