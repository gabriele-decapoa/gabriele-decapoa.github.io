---
layout: post
title: The core of al programming languages, or memory management
categories:
  - programming_languages
---
As you know, there are many programming paradigms: imperative, object-oriented, event-oriented, etc.
For each programming paradigm, you need to thing in a different way, to leverage the features of the paradigm the best possible.

From the computer point of view, there is no big difference between each paradigm.
During the standard behavior of your program, every function (method, subroutine, procedure, etc.) invocation define a new frame in memory stack.
If a function invokes a different one, a new frame was pushed in the stack.
Each frame contains function parameters, local variables and the pointer to the former frame.

All programming languages, whether C/C++, Java, Javascript, Python, etc., manage the memory as here described.
Just callback functions in Javascript have a slighter different mechanism, since the callback function resides in a different memory space called _callback stack_, but the frame is always the memory structure used to manage function data.
