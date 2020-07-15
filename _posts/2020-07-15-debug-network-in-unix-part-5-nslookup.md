---
layout: post
title: How to debug network issues in Unix - "nslookup"
categories:
  - bash
  - Unix
  - network
---

Restarting with my personal network debug toolkit, today I will describe `nslookup`.

`nslookup` is another tool that allows performing queries on domain name servers.  
As [Wikipedia](https://en.wikipedia.org/wiki/Nslookup) says,
> nslookup operates in interactive or non-interactive mode. 
> When used interactively by invoking it without arguments or when the first argument is `-` (minus sign) and the second argument is a hostname or Internet address of a name server, the user issues parameter configurations or requests when presented with the prompt (`>`). 
> When no arguments are given, then the command queries the default server. 
> The `-` (minus sign) invokes subcommands which are specified on the command line and should precede nslookup commands.  
> In non-interactive mode, i.e. when the first argument is a name or Internet address of the host being searched, parameters and the query are specified as command line arguments in the invocation of the program. 
> The non interactive mode searches the information for a specified host using the default name server.

So basically, `dig` and `nslookup` retrieves same information and will return them in different ways.

This is an example of `nslookup` operating in interactive mode.
```bash
$ nslookup

> www.google.com
Server:		192.168.0.1
Address:	192.168.0.1#53

Non-authoritative answer:
Name:	www.google.com
Address: 216.58.198.4
```

This is an example of `nslookup` operating in non-interactive mode (the stardard usage).
```bash
$ nslookup www.google.com
Server:		192.168.0.1
Address:	192.168.0.1#53

Non-authoritative answer:
Name:	www.google.com
Address: 216.58.198.4
```
