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

You could also query a specific DNS server using `dig @sever URL` syntax, or see all the calls performed using `dig +trace URL` syntax.
```
$ dig +trace www.nodered.org

; <<>> DiG 9.10.6 <<>> +trace www.nodered.org
;; global options: +cmd
.			483576	IN	NS	d.root-servers.net.
.			483576	IN	NS	j.root-servers.net.
.			483576	IN	NS	i.root-servers.net.
.			483576	IN	NS	l.root-servers.net.
.			483576	IN	NS	m.root-servers.net.
.			483576	IN	NS	c.root-servers.net.
.			483576	IN	NS	e.root-servers.net.
.			483576	IN	NS	k.root-servers.net.
.			483576	IN	NS	b.root-servers.net.
.			483576	IN	NS	f.root-servers.net.
.			483576	IN	NS	g.root-servers.net.
.			483576	IN	NS	h.root-servers.net.
.			483576	IN	NS	a.root-servers.net.
;; Received 239 bytes from 9.0.138.50#53(9.0.138.50) in 44 ms

org.			172800	IN	NS	d0.org.afilias-nst.org.
org.			172800	IN	NS	a0.org.afilias-nst.info.
org.			172800	IN	NS	c0.org.afilias-nst.info.
org.			172800	IN	NS	a2.org.afilias-nst.info.
org.			172800	IN	NS	b0.org.afilias-nst.org.
org.			172800	IN	NS	b2.org.afilias-nst.org.
org.			86400	IN	DS	9795 7 2 3922B31B6F3A4EA92B19EB7B52120F031FD8E05FF0B03BAFCF9F891B FE7FF8E5
org.			86400	IN	DS	9795 7 1 364DFAB3DAF254CAB477B5675B10766DDAA24982
org.			86400	IN	RRSIG	DS 8 1 86400 20190825050000 20190812040000 59944 . je1SWNWwQ1qR6emHZzzRj7t8LAmlxQEEIvHMNd6+O5gNt91Y7pH2X5MG EbPW2mNqDc6FBrGIhl+s6JIVTTtUyzmN9CSun15Cx8+SDHU4mtXKo3WR PXw/8+kAZ954+YzKfJ5EdHH2RBJCJtggK1eRVtLiIJTX+NKquD1/H8v/ 1ucsDI/z7qvAccnk3zK1yYTsZLuVQ0LDCL/ZrMzBmrj+fxLAYzSRGAe5 CjWzaIBUxzpzEtdpOhAsc4uHEonHY5iMb00Vro7dRTEqVW1mgPUw+aVI f29jsZneCo+7+yzI0aNDKKI+Yyi8nENxDMkogNW9BiN6VNcCbZszDX4W c30Aag==
;; Received 817 bytes from 2001:503:ba3e::2:30#53(a.root-servers.net) in 166 ms

nodered.org.		86400	IN	NS	matt.ns.cloudflare.com.
nodered.org.		86400	IN	NS	rafe.ns.cloudflare.com.
h9p7u7tr2u91d0v0ljs9l1gidnp90u3h.org. 86400 IN NSEC3 1 1 1 D399EAAB H9PARR669T6U8O1GSG9E1LMITK4DEM0T  NS SOA RRSIG DNSKEY NSEC3PARAM
h9p7u7tr2u91d0v0ljs9l1gidnp90u3h.org. 86400 IN RRSIG NSEC3 7 2 86400 20190902154211 20190812144211 44078 org. ggDUmLgHGFcctRfWnh/oIYXq/XcoXh753avQl2nFFJb10WePtNDlSQBz Ca7rIt+JDbKCco2EwDe8hLnSVMvUBfBigHcfudxzWXfMvdd3cpIONQoP mgYNq2tClQ/0yKahU+tpmtSzo+OakAstgZx9akqWJ4jRDrCGfxlN4Zya /2A=
l5kla3ec7t89nhvdgs2ap0gv4mhcp7tg.org. 86400 IN NSEC3 1 1 1 D399EAAB L5L0QRECKPN957E40C0KDPESHVU3QU07  NS DS RRSIG
l5kla3ec7t89nhvdgs2ap0gv4mhcp7tg.org. 86400 IN RRSIG NSEC3 7 2 86400 20190830152905 20190809142905 44078 org. egEr+azrmYo20+kszyiDC01cgq77JdDi8/5YKnKbjLjJzHtwSaSdM28g /+/DD12LGILgYu9X4Ufzk7v1X2MM41/WjUbwMKpOZRLvtg5GaVPNUoD9 m/Z+H23xwT6zy/9DCE3xCsIS3zrB2PXcD531+O1JsujNiLKbGuHq0bxq Ouk=
;; Received 592 bytes from 199.249.120.1#53(b2.org.afilias-nst.org) in 32 ms

www.nodered.org.	300	IN	A	104.27.163.46
www.nodered.org.	300	IN	A	104.27.162.46
;; Received 76 bytes from 173.245.59.131#53(matt.ns.cloudflare.com) in 32 ms
```
