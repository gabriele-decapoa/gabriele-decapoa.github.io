---
layout: post
title: How to debug network issues in Unix - "ping"
categories:
  - bash
  - Unix
  - network
---

After the [first part]({% link _posts/2019-08-12-debug-network-in-unix-part-1-dig.md %}), here a second command I frequently use to check network issues: `ping`.

With `ping` you could test, more or less, host reachability.
I am saying _more or less_ because it depends on the internals of `ping` and the configuration of host.  
As [Wikipedia](https://en.wikipedia.org/wiki/Ping_(networking_utility)) said
> Ping operates by sending Internet Control Message Protocol (ICMP) echo request packets to the target host and waiting for an ICMP echo reply. 
> The program reports errors, packet loss, and a statistical summary of the results, typically including the minimum, maximum, the mean round-trip times, and standard deviation of the mean.
So if the host's configuration blocks ICMP protocol, `ping` packet will be lost or rejected.

The behavior of `ping` command will be different among Unix/Mac OS and Windows system.  
On Windows, `ping` will automatically interrupt after 4 ICMP packages sent and received, while on Unix and Mac OS the command will continue to send ICMP packages until the programm will interrupt if there is no parameter toset threshold.

```
$ ping www.example.com
PING www.example.com (93.184.216.34) 56(84) bytes of data.
64 bytes from 93.184.216.34: icmp_seq=1 ttl=53 time=114 ms
64 bytes from 93.184.216.34: icmp_seq=2 ttl=53 time=116 ms
64 bytes from 93.184.216.34: icmp_seq=3 ttl=53 time=116 ms
64 bytes from 93.184.216.34: icmp_seq=4 ttl=53 time=540 ms
64 bytes from 93.184.216.34: icmp_seq=5 ttl=53 time=114 ms
64 bytes from 93.184.216.34: icmp_seq=6 ttl=53 time=115 ms
64 bytes from 93.184.216.34: icmp_seq=7 ttl=53 time=114 ms
64 bytes from 93.184.216.34: icmp_seq=8 ttl=53 time=115 ms
64 bytes from 93.184.216.34: icmp_seq=9 ttl=53 time=114 ms
64 bytes from 93.184.216.34: icmp_seq=10 ttl=53 time=115 ms
^C
--- www.example.com ping statistics ---
10 packets transmitted, 10 received, 0% packet loss, time 18224ms
rtt min/avg/max/mdev = 114.564/157.908/540.775/127.624 ms
```