---
layout: post
title: The magical world of containers - Linux background - Namespace
categories:
  - devops
  - containers
  - Unix
  - Cloud_development
---

This is a new post of this series about container world.

Here the list of previous posts.

* [Basic concepts]({% link _posts/2021-01-31-containers-world-1-basics.md %})
* [Benefits]({% link _posts/2021-03-13-containers-world-2-benefist.md %})

---

How a container achieve resource isolation?
There are some Linux features used by containerization in order to achieve this goal.  
First of all, let's focus on namespaces.

As [Wikipedia](https://en.wikipedia.org/wiki/Linux_namespaces) says, **namespace** is a Linux kernel's feature that allows to partition kernel resources.
Im that way, a processes' set sees one set of resources while another processes' set sees a different set of resources. 

The idea of namespace is common in programming world (e.g. in XML, C#, the packages in Java, etc.) and helps programmers to group commands in order to isolate themselves.
Same isolation is offered by Linux namespace, but a resource level.

Since kernel version 4.10, there are 7 kinds of namespaces.
* Segregation by process id (PID)
* Segregation by network stack
* Segregation by `cgroup` root directory
* Segregation by mount table
* Segregation by hostname and domain name
* Processes could communicate each other via IPC into namespace
* Segregation by user ID and group ID 

Summarizing what said in [this blog post from Medium](https://medium.com/@teddyking/linux-namespaces-850489d3ccf), without Linux namespace, containers cannot exists.
> Without namespaces, a process running in container A could, for example, umount an important filesystem in container B, or change the hostname of container C, or remove a network interface from container D. 
> By namespacing these resources, the process in container A isn’t even aware that the processes in containers B, C and D exist.

<!-- https://opensource.com/article/19/10/namespaces-and-containers-linux  >