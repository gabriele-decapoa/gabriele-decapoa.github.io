---
layout: post
title: curl tricks
categories:
  - devops
  - bash
---

BASH scripts are the most usefult things in DevOps world.  
Since the majority of enterprise system are currently UNIX/Linux systems, we could be sure that the same script could be run in many systems.

Sometimes, inside a BASH script you have to use `curl`: this command-line tool allows to get/send files using a plethora of Internet protocols (HTTP, HTTPS, FTP, etc.).  
The standard use of this tool does not allow to verify response code, but this could be useful during debug or if your script must run in an automated way.
So here a list of useful options you can use for those purpose:
- `-i` allows to visualize HTTP/HTTPS response headers
- `-v` make the output verbose --- you can put at most three `v` making the output as verbose as possible
- `-o`
- `-w`
