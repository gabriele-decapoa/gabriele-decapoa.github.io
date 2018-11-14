---
layout: post
title: Understanding asynchronous call in Node.js - callbacks and promises
categories:
  - Node_js
  - callback
  - promise
  - asynchronous
---

If programming languages you know are C-style (like Java or C++), so you know imperative and/or object-oriented programming, when you start to develop in Node.js you could be a little bit confused.

Synchronous call are predominant into these kind of languages, but not into Node.js. You *could* use synchronous functions, but Node.js' performance will be terrible.

Node.js is based on Javascript and it is created to develop mostly web applications, so performance and scalability are very important. You should understand very well asynchronous call to create a performant application in Node.js

## Callbacks

First approach with asynchronous function in Node.js are **callback** functions.

Usually, they are the last parameter of a function and they are used to get results of operations (typically I/O) that you want to have without blocking execution. A common callback function signature is

```javascript
callback(error, response)
```

It's very simple to create a callback function, and Node.js handles all the invocation of callbacks with its Event Loop, queueing a frame for each function and managing them one by one.
The drawback is the so-called *callback hell*.

```javascript
fs.readFile(nameFile, function(error, response) {
    readLine(response, function(error, line) {
        ...
    });
});
```
In the above example I've nested only two function, but if you write a lot of nested functions, your code will be unreadable.


## Promises

Starting from ECMAScript 6, Javascript allow to use a new construct: *promises*. A promise is like a Future in Java, an object that is a placeholder for the result.

Using promises you could write a more readable code, like this:

```javascript
fs.readFile(nameFile).then(function(response) {
    return readLine(response);
}).then(function(line){
    ...
}).catch(function(error){
    ...
});
```

Even if the code looks like synchronous, the behaviour is the same as using callbacks!
