---
layout: post
title: The importance to be logged
categories:
  - logging
  - Cloud_development
  - Best_practices
---
I was reading [this interesting article on IBM Cloud Blog](https://www.ibm.com/blogs/bluemix/2018/12/logging-and-error-handling-in-the-cloud/) and I started to think: "Damn, this guy is right!"  
Logs are very important for enterprise application, but understand how many log rows and at which level write them is the question.
Most of all, what I have to log and when?

What I saw as good practices in these years are:
1. `FATAL` log level should contains log info before crashes
2. use `DEBUG` log level to add a log line at the beginning and at the end of each function of your code helps during debugging
3. for same reasons, at the beginning of a function add a log line at `TRACE` level putting all meaningful parameters of the function
4. every time you catch an error, log it at `ERROR` level
5. do not abuse of `INFO` level: you have to use for expected behavior of your workflow, but if you log every single step _you do log nothing_
6. correlate every single log line, at each level, related to a single operation with a correlation ID
7. find a way, if the log library (or the programming language) you use does not allow it, to identify always the exact code line that throws the error

What are your best practices about logs?