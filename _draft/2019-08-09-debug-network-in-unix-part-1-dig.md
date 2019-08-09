---
layout: post
title: How to debug network issues in Unix - "dig"
categories:
  - bash
  - Unix
  - network
---

In my day-by-day job, I started to use lots of BASH commands to debug network issues, and I did never not many of them.
This is why I decided to write here all those commands and how do I use usually.

The first one I used frequently is `dig`.
As the man page stated,
> dig (domain information groper) is a flexible tool for interrogating DNS name servers. 
> It performs DNS lookups and displays the answers that are returned from the name server(s) that were queried.
> Most DNS administrators use dig to troubleshoot DNS problems because of its flexibility, ease of use and clarity of output.

Basically, you would use `dig` to check if there is some issue on resolving DNS records for the URL you are calling.

This is an example of `dig` usage.
```
$ dig www.nodered.org

; <<>> DiG 9.10.6 <<>> www.nodered.org
;; global options: +cmd
;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 16648
;; flags: qr rd ra; QUERY: 1, ANSWER: 2, AUTHORITY: 0, ADDITIONAL: 1

;; OPT PSEUDOSECTION:
; EDNS: version: 0, flags:; udp: 4096
;; QUESTION SECTION:
;www.nodered.org.		IN	A

;; ANSWER SECTION:
www.nodered.org.	300	IN	A	104.27.163.46
www.nodered.org.	300	IN	A	104.27.162.46

;; Query time: 81 msec
;; SERVER: 9.0.138.50#53(9.0.138.50)
;; WHEN: Fri Aug 09 17:34:44 CEST 2019
;; MSG SIZE  rcvd: 76
```