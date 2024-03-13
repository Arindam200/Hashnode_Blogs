---
title: "Unveiling Javascript Scopes"
datePublished: Sun Sep 24 2023 12:56:35 GMT+0000 (Coordinated Universal Time)
cuid: clmxgrgai000008l9agg0656z
slug: unveiling-javascript-scopes
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1690286604855/a31b1fb1-606e-4b0a-a9a4-cd5edfcdb3d3.png
tags: javascript, web-development, developer, reactjs, wemakedevs

---

## Introduction:

Scopes are one of the fundamental concepts of Javascript. Many Developers face problems understanding the behaviour of variables.

If you are also facing the same problem.

Don't Worry! You are in the right place!

In this blog, We'll explore What is Scope, and it's Types with examples.

## What is Scope?

> *Scope* in JavaScript refers to the current context of code, which determines the accessibility of variables to JavaScript.

We can only access the variables within their scopes otherwise they will throw an error.

Didn't Get it?

No worries!

Let's understand this with some examples.

```javascript
var Hero = 'Iron Man'

function Wish(){
    console.log(`My Favourite hero is ${Hero}`);
    var Villain = 'Thanos' 
} 
Wish(); //Invokes the function
console.log(`I was scared of ${Villain}`);
```

So what will be the Out of this code?

```javascript
// My Favourite hero is Iron Man
// Uncaught ReferenceError: Villain is not defined
```

Wondering why it's showing "Villain is not defined" in spite of declaring it?

Here comes the Scope.

Let me explain to you why this is happening,

We have defined the variable `Villain` with the function so the scope of the variable is within the function which means we can only access the variable within the function.

## Types of Scopes

There are 3 main types of scopes in JavaScript:

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1690267524457/e56d6bff-1ae7-4eb3-9982-0307b36f8871.png align="center")

### Global Scope:

If the variable is declared outside of a function then it has global scope. It can be accessed from anywhere in your JavaScript code.

```javascript
  let name = "Arindam"; // global scope

  function func1() {
    console.log(name); // Arindam
  }

  function func2() {
    console.log(name); // Arindam
  }
```

As the variable is declared globally, Both `func1` and `func2` can access the value and print Arindam.

### Function Scope:

A variable declared within a function has function scope. It can only be accessed within that function.

```javascript
  function func1() {
    let name = "Ronaldo"; // function scope

    console.log(name); // Ronaldo 
  }

  console.log(name); // Throws a ReferenceError
```

### Block Scope:

If you declare a variable with `let` and `const` , They are only accessible within the block they are declared in - between curly braces `{ }`.

Let's understand this with examples:

```javascript
  {
    let name = "Roni";
  }

  console.log(name); // Throws a ReferenceError
```

It throws a reference error because the scope of the variable is within the curly braces.

Now let's move to the next example,

```javascript
  {
    var name = "Messi";
  }

  console.log(name);
```

What will be the output here?

Undefined??

No, the output will be `"Messi"` .This is because the Variable defined with the `var` keyword is not block-scoped.

<div data-node-type="callout">
<div data-node-type="callout-emoji">💡</div>
<div data-node-type="callout-text">Note: Var is 'Function-scoped' whereas let and const are 'Block-scoped'</div>
</div>

## Conclusion:

If you found this blog post helpful, please consider sharing it with others who might benefit. You can also follow me for more content on Javascript, React, and other web Development topics.

For Paid collaboration mail me at : [arindammajumder2020@gmail.com](mailto:arindammajumder2020@gmail.com)

Connect with me on [Twitter](https://twitter.com/intent/follow?screen_name=Arindam_1729), [LinkedIn](https://www.linkedin.com/in/arindam2004/), [Youtube](https://www.youtube.com/channel/@Arindam_1729) and [GitHub](https://github.com/Arindam200).

Thank you for Reading :)

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1690284520449/09bb9835-905d-42c6-995c-5078ee2fed46.png align="center")