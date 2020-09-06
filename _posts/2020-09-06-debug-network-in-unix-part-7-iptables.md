---
layout: post
title: How to debug network issues in Unix - "iptables"
categories:
  - bash
  - Unix
  - network
---

Sometimes you could notice from some IPs you cannot receive any packet, or you could not reach out some IPs.
This means you probably have a firewall inside your machine or outside your machine which is blocking the network traffic.  
As we know, a **firewall** is a tool that allows to filter and manage network traffic a computer is listening.
Some firewall rules could be applied to block some protocols (e.g. TCP, UDP or ICMP), so you need to understand how those protocols works before setting any rules.  
To check any firewall inside your machine, you could use `iptables` command.

`iptables` is an administration tool for IPv4 packet filtering and for configuring NAT at Unix kernel level.  
In order to work, `iptables` needs **Netfilter**, a framework defined to manage network operations, to be enabled in the Unix kernel.  
Packets are grouped in three lists, called **chain**: `INPUT`, `OUTPUT`, `FORWARD`.
`INPUT` packets refers to _ingress_ (incoming) network connections, `OUTPUT` packets refers to _egress_ (outgoing) network connections and `FORWARD` packets refers to connections to manage and/or filter.

From a troubleshooting perspective, the first command you need to run is `iptables -L`, which will list all current rules enabled.
You could limit the output to a specifc chain specifing the name of the chain as parameter.

```bash
$ iptables -L INPUT
Chain INPUT (policy DROP)
target     prot opt source               destination
ACCEPT     all  --  anywhere             anywhere             ctstate RELATED,ESTABLISHED
ACCEPT     all  --  anywhere             anywhere
DROP       all  --  anywhere             anywhere             ctstate INVALID
UDP        udp  --  anywhere             anywhere             ctstate NEW
TCP        tcp  --  anywhere             anywhere             tcp flags:FIN,SYN,RST,ACK/SYN ctstate NEW
ICMP       icmp --  anywhere             anywhere             ctstate NEW
REJECT     udp  --  anywhere             anywhere             reject-with icmp-port-unreachable
REJECT     tcp  --  anywhere             anywhere             reject-with tcp-reset
REJECT     all  --  anywhere             anywhere             reject-with icmp-proto-unreachable
```

The first line of output indicates the chain name, followed by its default policy (DROP). 
The next line consists of the headers of each column in the table, and is followed by the chain’s rules.
* `target`: If a packet matches the rule, the target specifies what should be done with it (e.g. a packet can be accepted, dropped, logged, or sent to another chain to be compared against more rules)
* `prot`: The protocol, such as tcp, udp, icmp, or all
* `opt`: Rarely used, this column indicates IP options
* `source`: The source IP address or subnet of the traffic, or anywhere
* `destination`: The destination IP address or subnet of the traffic, or anywhere
The last column, which is not labeled, indicates the options of a rule. 
That is, any part of the rule that isn’t indicated by the previous columns.
This could be anything from source and destination ports, to the connection state of the packet.

It is also possible to show the number of packages matching any rule and the aggregate size of those packages, using also `-v` option.
```bash
$ iptables -L INPUT -v
Chain INPUT (policy DROP 0 packets, 0 bytes)
 pkts bytes target     prot opt in     out     source               destination
 284K   42M ACCEPT     all  --  any    any     anywhere             anywhere             ctstate RELATED,ESTABLISHED
    0     0 ACCEPT     all  --  lo     any     anywhere             anywhere
    0     0 DROP       all  --  any    any     anywhere             anywhere             ctstate INVALID
  396 63275 UDP        udp  --  any    any     anywhere             anywhere             ctstate NEW
17067 1005K TCP        tcp  --  any    any     anywhere             anywhere             tcp flags:FIN,SYN,RST,ACK/SYN ctstate NEW
 2410  154K ICMP       icmp --  any    any     anywhere             anywhere             ctstate NEW
  396 63275 REJECT     udp  --  any    any     anywhere             anywhere             reject-with icmp-port-unreachable
 2916  179K REJECT     all  --  any    any     anywhere             anywhere             reject-with icmp-proto-unreachable
    0     0 ACCEPT     tcp  --  any    any     anywhere             anywhere             tcp dpt:ssh ctstate NEW,ESTABLISHED
```

Once you identified any rule that could stop the connection you need, then you need to identify whether that rule is mandatory or not and eventually delete if not mandatory.