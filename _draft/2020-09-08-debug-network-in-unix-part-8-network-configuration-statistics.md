---
layout: post
title: How to debug network issues in Unix - network configuration and statistics
categories:
  - bash
  - Unix
  - network
---

Another sets of useful CLI commands for network issues debugging are related to the network configuration from a network interface point of view and also from a ARP table point of view.  
There are few commands to performs those debug actions, but in newer Unix version all those commands where replaced by a single command called `ip`.  
In this post we will describe some basic usage for each command.
For further details, please have a look on _man pages_.

### `ifconfig`
The basic command you could use to list all network interfaces and have eventually the IP address associated to that interface is `ifconfig` (which correspond to `ipconfig` in Windows world).

Standard usage is to list current network interfaces' configuration.
```bash
$ ifconfig -a
lo0: flags=8049<UP,LOOPBACK,RUNNING,MULTICAST> mtu 16384
	options=1203<RXCSUM,TXCSUM,TXSTATUS,SW_TIMESTAMP>
	inet 127.0.0.1 netmask 0xff000000 
	inet6 ::1 prefixlen 128 
	inet6 fe80::1%lo0 prefixlen 64 scopeid 0x1 
	nd6 options=201<PERFORMNUD,DAD>
eth0: flags=8863<UP,BROADCAST,SMART,RUNNING,SIMPLEX,MULTICAST> mtu 1500
	options=400<CHANNEL_IO>
	ether xx:xx:xx:xx:xx:xx 
	inet6 xxxx.xxx.xxx..xxxxx%en0 prefixlen 64 secured scopeid 0x4 
	inet 192.1.0.111 netmask 0xffffff00 broadcast 192.0.0.255
	nd6 options=201<PERFORMNUD,DAD>
	media: autoselect
	status: active
```

It is possible to use this command to turn on and off a network interface.
For example, to turn off the network interface named `eth0` you need to run `ifconfig eth0 down`, while to turn on you need to run `ifconfig eth0 up`.  

### `netstat` and `ss`
If you would like to check network protocols' statistics, `netstat` is the tool you need to run.
Even if this command is quite obsolete, it is still used to determine the amount of network traffic, helping on debug issues.

```bash
$ netstat -a | more

Active Internet connections (servers and established)
Proto Recv-Q Send-Q Local Address               Foreign Address             State
tcp        0      0 *:sunrpc                    *:*                         LISTEN
tcp        0     52 192.168.0.2:ssh             192.168.0.1:egs             ESTABLISHED
tcp        1      0 192.168.0.2:59292           www.gov.com:http            CLOSE_WAIT
tcp        0      0 localhost:smtp              *:*                         LISTEN
tcp        0      0 *:59482                     *:*                         LISTEN
udp        0      0 *:35036                     *:*
udp        0      0 *:npmp-local                *:*

Active UNIX domain sockets (servers and established)
Proto RefCnt Flags       Type       State         I-Node Path
unix  2      [ ACC ]     STREAM     LISTENING     16972  /tmp/orbit-root/linc-76b-0-6fa08790553d6
unix  2      [ ACC ]     STREAM     LISTENING     17149  /tmp/orbit-root/linc-794-0-7058d584166d2
unix  2      [ ACC ]     STREAM     LISTENING     17161  /tmp/orbit-root/linc-792-0-546fe905321cc
unix  2      [ ACC ]     STREAM     LISTENING     15938  /tmp/orbit-root/linc-74b-0-415135cb6aeab
```

Anyway, as said before, this command is obsolete.
In newest Unix versions, there is a new command that replace `netstat`: `ss`.
```bash
$ ss | head -n 5
Netid  State      Recv-Q Send-Q Local Address:Port      Peer Address:Port
u_str  ESTAB      0      0       * 23740                * 23739
u_str  ESTAB      0      0       * 23707                * 23706
u_str  ESTAB      0      0       * 87021                * 88383
u_str  ESTAB      0      0       * 17056                * 17112
```

### `arp`
Sometimes you need to analyze and manipulate ARP local cache.
Unix operating system allows to do that using `arp`.

```bash
$ arp
Address                  HWtype  HWaddress           Flags Mask            Iface
192.157.175.1            ether   00:50:55:c0:00:07   C                     eth0
192.157.175.2            ether   00:50:55:fd:b2:1a   C                     eth0
192.157.175.254          ether   00:50:55:e5:7d:12   C                     eth0
```

### `routes`
To view and manage the IP routing table at kernel level, you could use `route` command.

```bash
$ route
Kernel IP routing table
Destination     Gateway         Genmask         Flags Metric Ref    Use Iface
default         192.168.1.2     0.0.0.0         UG    1024   0        0 eth0
192.168.1.0     *               255.255.255.0   U     0      0        0 eth0
```

### `ip`
As said at the beginning, latest version of Unix systems offers a new, single command to perform all those network analysis: `ip`.
`ip` command in Linux is present in the `net-tools` package, which is used for performing several network administration tasks.

Let's see the equivalent commands for the aforementioned ones.

| Old style | New style |
| --------- | --------- |
| `arp -a` | `ip neigh` |
| `arp -v` | `ip -s neigh` |
| `arp -s 192.168.1.1 1:2:3:4:5:6` | `ip neigh add 192.168.1.1 lladdr 1:2:3:4:5:6 dev eth1` |
| `arp -i eth1 -d 192.168.1.1` | `ip neigh del 192.168.1.1 dev eth1` |
| `ifconfig -a` | `ip addr` |
| `ifconfig eth0 down` | `ip link set eth0 down` |
| `ifconfig eth0 up` | `ip link set eth0 up` |
| `ifconfig eth0 192.168.1.1` | `ip addr add 192.168.1.1/24 dev eth0` |
| `ifconfig eth0 netmask 255.255.255.0` | `ip addr add 192.168.1.1/24 dev eth0` |
| `ifconfig eth0 mtu 9000` | `ip link set eth0 mtu 9000` |
| `ifconfig eth0:0 192.168.1.2` | `ip addr add 192.168.1.2/24 dev eth0` |
| `netstat` | `ss` |
| `netstat -neopa` | `ss -neopa` |
| `netstat -g` | `ip maddr` |
| `route` | `ip route` |
| `route add -net 192.168.1.0 netmask 255.255.255.0 dev eth0` | `ip route add 192.168.1.0/24 dev eth0` |
| `route add default gw 192.168.1.1` | `ip route add default via 192.168.1.1` |