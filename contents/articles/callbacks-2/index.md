---
title: Callbacks in JavaScript - Part 2
author: ritwik
date: 2014-02-19
template: article.jade
---

In the previous article we looked at what callback functions are and a little into JavaScript functions in general. Here we'll explore, in some detail, the reasons behind using callbacks.

In web programming, it is important to know that browsers provide a single thread for everything accessing the user interface. This means that all the application logic accessing, fetching data and modifying the user interface elements is always in the same thread. In synchronous code this would imply that for any operation to execute it would have to wait for the prior one to complete and would render the UI unresponsive to the user in case of long running tasks. This is generally considered as a bad design decision in modern web applications. All modern browsers provide for asynchronous capabilities (APIs) to circumvent issues like the above.

Most of you would have heard of the term AJAX (Asynchronous JavaScript and XML). But what is it that makes this technology asynchronous? It uses the XMLHttpRequest object to accomplish this. This object, first implemented by Microsoft as an ActiveX object but now also available as a native object within all modern browsers, enables JavaScript to make HTTP requests to a remote server without the need to reload the page. In essence, HTTP requests can be made and responses received, completely in the background and without the user experiencing any visual interruptions.

<span class="more"></span>

```
// Create the XHR object to do GET to /data resource  
var xhr = new XMLHttpRequest();
xhr.open("GET","data",true);

// Register the event handler
xhr.addEventListener('load',function(){
  if(xhr.status === 200){
      alert("We got data: " + xhr.response);
  }
},false) 

// Perform the work
xhr.send();
```

The above is an example of event based asynchronous implementation, although, we do pass in a function to be called when the "load" event completes.

Other browser APIs, such as SQLite and HTML5 Geolocation, are callback based, meaning that the developer passes a function as argument that will get called back by the underlying implementation with the corresponding resolution.

For example, for HTML5 Geolocation, the code looks like this:

```
// Call and pass the function to callback when done.
navigator.geolocation.getCurrentPosition(function(position){  
            alert('Lat: ' + position.coords.latitude + ' ' +  
                  'Lon: ' + position.coords.longitude);  
});
```  

The basic difference between callback based and event based execution is in whether the developer is aware of when a function call / code execution is initiated. A button click by the user can occur at any time without prior indication. We call the "click" an event and use event handlers / listeners to handle this. In callback based asynchronous code, the entire implementation is under the control of the underlying process. That is, the developer is aware of an operation that he requires be executed and passes it a function that may be called at any desired point within the executing operation. It's all very subtle.

> Consider using callbacks to allow users to provide custom code to be executed by the Framework.

> Consider using events to allow users to customize the behavior of a framework.

So far, we have seen the use of callback functions in async programming. However, not all callbacks are asynchronous. In fact, asynchronous capabilities / events are provided only by the browser.

The following code excerpt from the last article is a synchronous callback execution

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

So how is the above useful? We may imagine a scenario where there are multiple operations that differ in only a single execution step. Instead of writing multiple implementations that contain repetitive code we can define a template function that contains the common execution steps and accepts a callback function as an additional parameter. We will call this function at the point where my code execution differs based on the operation. Depending on the scenario, we can pass in an appropriate function that mimics the single differing execution step. This decreases redundancy and code size. The role of the callback is to provide a partial implementation that can be easily swapped in a larger algorithm.

This discourse mirrors my understanding of the use of callback functions in programming. It is also a peek into the philosophy of code.