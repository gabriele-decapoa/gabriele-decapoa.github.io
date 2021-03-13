---
layout: post
title: The magical world of containers - benefits
categories:
  - devops
  - containers
  - Unix
  - Cloud_development
---

This is a new post of this series about container world.

Here the list of previous posts.

* [Basic concepts]({% link _posts/2021-01-31-containers-world-1-basics.md %})

---

Why one should adopt containers in his/her software architecture?  
As said before, _containers_ is a buzz word, but the technical stuffs behind this concepts have lots of benefits.

#### Agile application creation and deployment
Creating a container image is easier and more efficient compared to create a virtual machine template, even if some softwares like _Vagrant_ or _Terraform_ made easier virtual machine template's creation.

#### Continuous development, integration, and deployment
Continuous Development, Continuous Integration and Continuous Deployment methodologies are simplified.  
One could build frequently and reliably new container images with new software version and one could deploy new versions and perform, if needed, the rollback to previous version easily and quickly.

#### Dev and Ops separation of concerns
One could create application container images at build/release time rather than deployment time, thereby decoupling applications from infrastructure.

#### Environmental consistency
Runs the same on a laptop as it does in the cloud across development, testing and production environments, so this resolve the age old dilemma "_... but it works on my pc!_"

#### Cloud and OS distribution portability
Containers guarantees interoperability among operatings systems and cloud providers.
Thanks to its standard, same container images could run on Linux, on Mac OS X, on Windows (with some tricks), but also on Google Container Engine, IBM Cloud Kubernetes Engine, Amazon Elastic Kubernetes Service, etc.

#### Application-centric management
Containers help to Raise the level of abstraction from running an OS on virtual hardware to run an application on an OS using logical resources.

#### Loosely coupled and distributed micro-services
As soon as containers became a buzz word, the _microservice_ architectural pattern was defined.
Thanks to containers, an application is broken into smaller, independent pieces and can be deployed and managed dynamically.

#### Resource isolation and utilization improvement
Thanks to namespaces and jailing, resources allocated to a single container will be completely separated from the resources allocated to another container.
In addition, namespace and jailing allows to define a threshold, improving also the utilization rate for the resources.