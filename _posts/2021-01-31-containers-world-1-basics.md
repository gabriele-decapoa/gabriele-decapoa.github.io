---
layout: post
title: The magical world of containers - basic concepts
categories:
  - devops
  - containers
  - Unix
  - Cloud_development
---

Anyone in my LinkedIn network knows last year I became a Certified Kubernetes Application Developer.
After about 4 years working actively on Kubernetes day by day, this certification was a great recognition of my studies.  
This is why I decided to start a new post series, describing what I've learned about containers and their use cases.
Before starting, I would add a **caveat**: this is what I understood after studying on books and on the job, but could include lots of misunderstanding, so please use those posts just a starting point to deepen your knowledge, starting your own learning roadmap!

---

First of all, let's start with the basics: _what is a container_?   
The term _container_ became a buzz word in last 15 years.
It seems everyone uses containers and who don't uses a container doesn't innovate.
Anyway, if you ask to those people explaining what technically is a container, no one could share a simple definition.  
What I understood, a container is a logical object into Unix operating systems, called _controlled user process_.
This process could hide to the kernel one or more real processes.

Usually containers are compared to virtual machines, but they are not so similar.  
Virtual machines isolate applications using _hypervisor_, virtualizing hardware and the whole operating systems, while containers isolate application using virtual memory, namespaces and jailing of host kernel.
Virtual machines are very slow and expensive to be created, and their portability are not so easy.
Containers are very fast, their creation are cheaper than VMs.  
Finally, containers are more secure than virtual machines.
When you talk about container security, in fact, you shouldn't be worried about the management of this user process, but just about how the process is isolated.  
Container security is guaranteed creating isolation layers between applications and between the application and host.
Those layers reduce the host surface area which protects both the host and the co-located containers by restricting access to the host.

On the other side, containers could be compared to processes, but also in this case they are not so similar.
A process is a representation of a running program, while the container is a group of processes, isolated, managed by shared kernel and visible as a single process.

What about the history of the concept of the container?  
From 1979 and 1982, operating systems programmers introduced a new system call before Unix V7 and then BSD: the `chroot`.
`chroot` (_change root_) changes the apparent root directory for the current running process and its children.  
This way to isolate processes was the only one until early 2000s, when was introduced _jail_ mechanism in FreeBSD and Linux VServer operating systems.
Jailing achieves a clear-cut separation between services and customers for security and ease of administration.  
It was only in the 2004 the word _containers_ was used in IT world.
In Solaris operating systems, containers combine system resource controls and boundary separation provided by zones.
In 2006, thanks to the studies of IBM and Google, process containers (aka `cgroups`) were added into Unix operating systems; their containers were designed for limiting, accounting and isolating various resources usage (e.g. CPU, memory, disk I/O, network) for a collection of processes.   
In 2008, _Linux Containers_ (LXC) were introduced.
Those kind of isolation were composed based on `cgroups` and Linux namespaces and it works on a single Linux kernel without requiring any patches.

This was a brief history, not completed, that defines the main steps of the container evolution.  
In next posts we deeply describe each of those steps.