---
layout: post
title: Logging in Node.js for enterprise application
---

There are many packages that allows logging into a Node.js application: [winston](https://github.com/winstonjs/winston), [morgan](https://github.com/expressjs/morgan), etc.

However, for an enterprise application, these libraries, in my honest opinion, expose a set of features too little
 For this reason I prefer to use the Node.js version of [Apache Log4j] (https://logging.apache.org/log4j/2.x/) library: [`log4js-node`](https://github.com/nomiddlename/log4js-node).
As the readme explain, this package is very simple to use:

```Javascript
var log4js = require('log4js');
var logger = log4js.getLogger();
logger.debug("Some debug messages");
```

In addition, this library allows to write log message using different log level (INFO, DEBUG, WARN, etc.), so it is possible to run an application in production environment removing all log messages useful during development without removing them from source code.
