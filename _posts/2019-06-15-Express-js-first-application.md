---
layout: post
title: How to create your first web application with Node.js and Express
categories:
  - Node_js
  - programming_languages
---

As we see in [this article]({% link _posts/2019-05-07-Node-js-first-application.md %}), [Node.js](https://nodejs.org/en/) is a language very powerful, but it has not an application server embedded.  
To expose your application on the Internet, you need an application server, and the most used, well documented and simple is Express.

[Express](https://expressjs.com) is a minimal and flexible Node.js web application framework that provides a robust set of features for web and mobile applications.  
This framework provides a thin layer of fundamental web application features, without obscuring Node.js.

To use Express you just
* insert `"express" : "<version>"` into `dependencies` section of `package.json` file and then execute `npm install`, or
* run `npm install express –save` command.

After that you could start to write your code and you have a simple Node.js application that uses Express.

```node
var express = require(’express’);
var app = express();

app.get(’/’, function(req , res) {
  res.send(’Hello World!’);
});

app.listen(8000, function() {
  console.log(’Example app listening on port 8000!’);
});
```

This is the same identical application we saw in the previous article, but it is more concise.
Express, in fact, create itself an HTTP server, so it is no more necessary to use `http` module.
