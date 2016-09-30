---
title: I Promise To ...
excerpt: "Give you what you want... maybe... actually probably not..."
teaser: promise.jpg
header:
  overlay_image: promise.jpg
  overlay_filter: 0.5
---

# Introduction

We've talked a couple of times about Javascript's asynchronicity, and today I want to write a little about one of the ways we can cope without resulting to callbacks. Really, I promise this will be really interesting.

A Promise object is used for asynchronous computations and represents a value which may be available now, or in the future, or never. It allows you to define functions to be called on an asynchronous action's eventual completion.

One of the archetypal asynchronous actions is a AJAX request. If you've forgot about these since the last time we talked about them, I wrote a short introduction [here](https://tomstuart92.github.io/Ajax/) which you may find useful.

# A simple request

For all those who feel comfortable as AJAX warriors, lets write a method that makes get requests to the url we provide:

```javascript
function getURL(url) {
  var request = new XMLHttpRequest();
  request.open("GET", url, true);

  request.onload = function() {
    if (request.status == 200) {
      console.log(request.response);
    }
    else {
      throw 'There was an error';
    }
  };
  request.send();
}
```

This is a simple ajax request that opens a GET request to the given URL, and then defines a callback for when the response loads up which checks the status code of the response and then logs the response string.

This is fine if we only want to print out the response string to the console, but for any real world application, we'll probably want to pass the result of this onto another function to do something more interesting. Perhaps printing to the page, or transforming the data into another format.

This will create an issue if we just return the response body and then try to pass on the result to our next function. The requests will go off, but because we don't wait for them, the code will continue executing and the next function will be called before the data becomes available.

To solve this problem we turn to the topic of our discussion, promises.

# Promises, Promises, Promises

Lets re-write that piece of code with a promise:

```javascript
function getURL(url) {
  return new Promise(function(resolve, reject) {
    var request = new XMLHttpRequest();
    request.open("GET", url, true);

    request.onload = function() {
      if (request.status == 200) {
        resolve(request.responseText);
      }
      else {
        reject(Error('There was an error'));
      }
    };
    request.send();
  });
}
```

In this iteration the object that gets returned from our getURL method is a `Promise` object. The promise creation takes two arguments, resolve and reject, which broadly corresponds to success and failure. We wrap our asynchronous operation in this promise object and then we wrap the successful return object in a resolve block, and any errors in a reject block.

The promise object has four possible states:

- Fulfilled: The action relating to the promise succeeded
- Rejected: The action relating to the promise failed
- Pending: Hasn't fulfilled or rejected yet
- Settled: Has fulfilled or rejected

When the request is sent it is pending, and then when the response comes back it moves to be settled and either fulfilled or rejected.

The true power of promises comes with the use of the `promise.then` method. This allows us to make method calls on the return value of our promise and have the program wait for the promise to be settled.

For example if we wanted to parse the return value of our response into a JSON, we could do the following:

```javascript
getURL("http://www.targetAPI.com").then(function(response) {
  JSON.parse(response);
}, function(error) {
  console.log(error)
});
```

If the function passes with a fulfilled code, the first function gets called and we parse the response. If the AJAX call fails we log the error. The important point though is that the code in the `.then()` block does not get executed until the promise has been resolved.

This is a really useful tool for dealing with asynchronous calls in JS, and very powerful. We can have multiple promises, chained `.then` blocks and other funky structures as needed to give exactly the behaviour we need. 
