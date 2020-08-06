---
layout: post
title: How to debug network issues in Unix - "traceroute" and "mtr"
categories:
  - bash
  - Unix
  - network
---

A new chapter in my personal network debug toolkit: network hop analysis.

Sometimes your packets seem to be lost somewhere in the Net and you would like to understand exactly where.
Analyzing each network _hop_, so each router in which the packet will be transfer to, usually you could find the bottleneck.  

The simplest command for that is `traceroute` (which Windows version is `tracert`).  
`traceroute` will send packets across the Net and will measure transit delays (also known as _round-trip delay_) to each router, calculating also all possible paths.
The protocol used by default depends on the operating system: Unix version uses UDP, while Windows uses ICMP.  
The usage is very simple:
```bash
$ traceroute -w 3 -q 1 -m 16 example.com
traceroute to example.com (93.184.216.34), 16 hops max, 52 byte packets
 1  192.168.0.1 (192.168.0.1)  3.303 ms
 2  151.6.149.72 (151.6.149.72)  9.011 ms
 3  151.7.92.68 (151.7.92.68)  9.671 ms
 4  151.6.84.132 (151.6.84.132)  9.303 ms
 [...]
```
In the example above, selected options are to wait for three seconds (instead of five), send out only one query to each hop (instead of three), limit the maximum number of hops to 16 before giving up (instead of 30), with example.com as the final host.

Another useful tool is `mtr` (which Windows version is `WinMTR`).  
`mtr` is more powerful, as combine `traceroute` and `ping` features.
As [man page](https://linux.die.net/man/8/mtr) stated
> As `mtr` starts, it investigates the network connection between the host `mtr` runs on and an external hostname, by sending packets with purposly low TTLs. 
> It continues to send packets with low TTL, noting the response time of the intervening routers. 
> This allows mtr to print the response percentage and response times of the internet route to the external hostname. 
> A sudden increase in packetloss or response time is often an indication of a bad (or simply overloaded) link.

An example of `mtr` usage is the following:
```bash
$ mtr -b google.com

Start: Thu Jun 28 12:14:36 2018
HOST: TecMint                     Loss%   Snt   Last   Avg  Best  Wrst StDev
  1.|-- 192.168.0.1                0.0%     5    0.3   0.3   0.3   0.4   0.0
  2.|-- 5.5.5.211                  0.0%     5    0.7   0.8   0.6   1.0   0.0
  3.|-- 209.snat-111-91-120.hns.n  0.0%     5    1.4   1.6   1.3   2.1   0.0
  4.|-- 72.14.194.226              0.0%     5    1.8   2.1   1.8   2.6   0.0
  5.|-- 108.170.248.209            0.0%     5    2.0   1.9   1.8   2.0   0.0
  6.|-- 216.239.56.115             0.0%     5    2.4   2.7   2.4   2.9   0.0
  7.|-- bom07s15-in-f14.1e100.net  0.0%     5    3.7   2.2   1.7   3.7   0.9
```