---
layout: post
title: Git internals
categories:
  - Cloud_development
  - devops
  - git
---
[Git](https://git-scm.com/), after its first release in 2005 thanks to Linus Torvalds, became rapidly the most source code management (SCM) tool used in the world, expecially for open source projects.  
This was due to its design.
Torvals, in fact, develop this system to be very stupid, fast and simple, because it has to support Linux kernel development.
So, other featurse added were the strong support for non-linear development and for large projects.

Git, as we know, is a fully distributed SCM, and each developer that works on that project has in his/her machine a full copy of the repository into the hidden directory `.git\`.

But how data are stored internally?

![Git snapshot](/images/Git-snapshot.jpg)
*
Git thinks of its data like a set of *snapshots* of a miniature filesystems.
If a file has not changed, Git does not store it again but create a link to the previous version.
Everything is check-summed before it is stored and it is referred to by that checksum.  
Each snapshot (**commit**) refers to one or more other snapshot, creating a [*directed acyclic graph*](https://en.wikipedia.org/wiki/Directed_acyclic_graph) (DAG).

This DAG is immutable and append-only, and could be view like an _object store_, in which we have:
* *blob* (binary large object) - the content of a file
* *tree* -  list of file names, each with some type bits and a reference to a blob or tree object that is that file, symbolic link, or directory's contents
* *commit* - links tree objects together into a history
* *tag* - container that contains a reference to another object and can hold added meta-data related to another object