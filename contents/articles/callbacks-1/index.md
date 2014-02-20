---
title: Callbacks in JavaScript - Part 1
author: ritwik
date: 2014-02-06
template: article.jade
---

If you're reading this, you've probably heard about them. May be used them too. Or maybe not. But what exactly are callback functions? Why do we use them? Do we understand the premise of their existence in programming today?

I recently solved a minor issue at work using a simple setTimeout() method in JavaScript. Honestly, it was just putting in a delay in my code execution to allow the system to execute its own function first. Simple as it is, I failed to make it work the very first time. The problem was with the way I had passed my callback function to setTimeout() and this triggered a switch that led me to refresh my understanding of functions in JavaScript.

####How it all began

My code:

```
setTimeout(MyFunction(), 3000);

function MyFunction() {
    //Some logic
};

```
<span class="more"></span>

Now, the setTimeout() function takes two arguments: a **function reference** (the function to be executed after the given time interval) and an interval in milliseconds. Above, however, you will notice that I'm actually passing in the **return value** of MyFunction() to setTimeout(). The parentheses after the function name in JavaScript indicate that the function is to be executed immediately.  

The correct syntax:

```setTimeout(MyFunction, 3000);```

OR (using an anonymous function)

```
setTimeout(function() {
    //Some logic
}, 3000);
```
Infact, the following is also perfectly alright:

```
function MyFunction(param) {
    return function() {
        // does something with param
    }
}

setTimeout(MyFunction(), 3000);
```
It has nothing to do with how setTimeout() receives a function as its argument, but that it does.

####Callbacks

The important thing of note here is that JavaScript treats functions as "first class" objects. Specifically, this means JavaScript supports passing functions as arguments to other functions, returning them as the values from other functions and assigning them to variables.

A callback is a piece of executable code (function) that is passed as an argument to a containing function, which executes the argument at any specified point.

So, for example:

```
function foo() {
    alert(this);
}

function bar(A, callback) {
    alert(A*A);
    callback.call(A);
}

bar(5, foo);
```

The above code will execute to give me two consecutive pop-ups. The first will display 25, the second 5. Here foo is passed as the callback function to bar.

You will notice that I've made use of the ***this*** keyword in foo. Also, I wrote callback.call(A) inside bar. Had I simply written callback(A) instead, my second alert would display *[object Window]*. This happens as a result of how the value of the *this* keyword is bound in JavaScript. 

When using the **call** or **apply** methods of Function.prototype, the value of *this* inside the called function gets explicitly set to the first argument of the corresponding function call.

Only writing callback(A) sets the value *this* to the global object, which is *Window*.

Now, from the above example we know how to use a callback in a  function. But we could have performed the identical operations had we simply invoked foo() from within bar(). Where, then, is the benefit of passing functions as first class objects? This was the question that I asked myself that day at work. And this is where it led me.

So we know the "what". And a bit of the "how". In my next post we'll try and answer the more important question - that of its philosophy. That of its quintessential "why". 