---
layout: post
title: How to create your first application in Node.js
categories:
  - Node_js
  - programming_languages
---

[Node.js](https://nodejs.org/en/) is a language very powerful.  
Written initially in 2009, it became rapidly one of the most used languages in cloud development world due to some factors.
* It is based on Javascript, which is a language with a small learning curve
* It is asynchronous
* Well documented
* Big library public registry (known as [npm](https://www.npmjs.com))
* It is data intensive, lightweight and perfect on distributed devices

Unfortunately, the fact it is asynchronous it also one of the biggest blockers to the use of this language. 
For whom is coming from the sequential, imperative world, understanding the asynchronous logic could be difficult.  
In addition, the fact it is based on Javascript, and built on Chrome’s V8 engine, even if the engine is written in C++ Node.js is not suitable for high computational programs.
Anyway, if you are looking for a fast prototype development, or a web app, this language is perfect.


With few code lines, you could easily create a naive web application that returns an "Hello World" on the root path.
```node
// Load the http module to create an http server.
var http = require ('http');

// Configure our HTTP server to respond with Hello World to all requests.
var server = http.createServer(function(request,response){
    response.writeHead(200,{"Content-Type":"text/plain"});
    response.end("Hello World\n");
});
// Listen on port 8000, IP defaults to 127.0.0.1
server.listen(8000);

// Put a friendly message on the terminal
console.log("Server running at http ://127.0.0.1:8000/");
```