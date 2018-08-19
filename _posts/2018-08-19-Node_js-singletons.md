---
layout: post
title: Improving memory footprint in Node.js: using Singleton pattern
categories:
  - Node_js
---

In my job I use Node.js extensively as back-end language in a event-driven architecture.
The application works so well, until the event load increased too much and the app started to crash periodically with _Out of memory_ reason.

After a first shocking reaction (in two years the application never crashes for memory outage), with my colleagues I started to analyze the memory footprint of our application.
The first well known technique is [memory profiling](https://www.google.it/search?q=node+js+memory+profiling&oq=nodejs+memory+profi&aqs=chrome.3.69i57j35i39j0l4.9174j0j7&sourceid=chrome&ie=UTF-8), with heap dump and snapshots, trying to replicate in our machines same conditions we noticed for memory crashes, but it is very difficult to understand an heap dump.
After that, we started to analyze statically application' source code to find something could explain this crash, so we found that.

For each event type of our architecture, the application defines a different ECMAScript6 class; nothing to say, it is the right choice because each event type needs to be managed in a different way. Each class is defined in a different module, and exposed as
```javascript
module.exports = EventTypeClass
```
In this way, for each event the application receives, we need to create a new instance of `EventTypeClass`. Since the events flow rapidly, Node.js garbage collector seems to not clean up all _dead_ instances (as soon as the event is managed, the instance reference to root obejct was deleted), so the application crashes.

Here the [Singleton pattern](https://en.wikipedia.org/wiki/Singleton_pattern) came to help. As Wikipedia said, and all software developer knows,
>  the singleton pattern is a software design pattern that restricts the instantiation of a class to one object.

Knowing how Node.js module caching mechanism works, we decided to change how we expose the class:
```javascript
module.exports = new EventTypeClass()
```
In this way, we expose the instance of the class, and since we `require` modules only one time (at application bootstrap), we have replicated the Singleton pattern.

As soon as we deployed new application version in production, we noticed no more crashes and a reduced memory footprint.

By the way, as explained [in this article](https://medium.com/@lazlojuly/are-node-js-modules-singletons-764ae97519af), applying this solution in every case does not grant you will create singleton classes, so I suggest to read the article and find your personal way to define singleton classes for your use cases.
