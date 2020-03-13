---
layout: post
title: Node.js development best practices
categories:
  - Best_practices
  - Node_js
---

In past I wrote about [how to write a simple Node.js application]({% link _posts/2019-05-07-Node-js-first-application.md %}).
Developing a Node.js application is simple, but if debugging it could be very difficult, in particular if your application is running on a cloud environment.  
This is why I found some useful best practices for development that could simplify the debug.

## Use fixed dependencies version
In `package.json` file, as you could read [here](https://docs.npmjs.com/files/package.json), you could define the version of the dependencies in multiple ways, via exact match or fuzzy match (i.e. greater than, compatible).  
In early versions of Node.js runtime, using the fuzzy match could create lots of issues during debugging: you could use a different dependency version in local machine and in production environment.
This is why starting from version 8, Node.js introduced [`package-lock.json`](https://docs.npmjs.com/configuring-npm/package-lock-json) file, that allow to lock specific version once installed in local environment.  
A best practice is to commit the `package-lock.json` file in your source code repository, or use exact match for dependency.

## Extensive log

## Test, test, test!
