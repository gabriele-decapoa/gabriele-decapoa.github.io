---
layout: post
title: Understanding asynchronous call in Node.js - async and await
categories:
  - Node_js
  - asynchronous
---

As explained in [this post](https://gabriele-decapoa.github.io/2017/04/17/Understanding-asynchronous-call-in-Node_js-callbacks-and-promises/), Node.js' performance will be very good if the source code this framework will run is asynchronous.
This is why, starting from version 6, ECMAScript language (the "father" of Javascript, and then of Node.js) adds some "programmer's help" to make more easy-to-read an asynchronous code (e.g promises).  
Starting from ECMAScript 8, a new syntax was added: `asynch` and `await`.

`asynch` is a keyword to be added in function declaration and mean that the function is asynchonous, while `await` is a keyword to be used when an asynchonous function will be invoked, allowing to assign function result to a variable like synchronous functions.  
The only _cons_ about using these two keywords is the use of try-catch block to catch errors, despite promises' catch function.

So, what with promises will be written like
```javascript
fs.readFile(nameFile).then(function(response) {
    return readLine(response);
}).then(function(line){
    ...
}).catch(function(error){
    ...
});
```
with asych and await will be written like
```javascript
aynch function readFile(content) {
  ...
}


try {
  const response = await fs.readFile(nameFile);
  const line = await readLine(response);
  ...
} catch (error) {
  ...
}
```