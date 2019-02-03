---
layout: post
title: The 12-factor app for dummies
categories:
  - Cloud_development
  - Best_practices
  - 12-factor
---

One of the best methodologies for software development I have seen in last years is the [12-factor app](https://12factor.net).  
Even if most of those factors are already known as best practices in software development world, this is the first methodology that summarize how to develop an enterprise application.

Let us analyze step by step this methodology.

### 1. One codebase tracked in revision control, many deploys
Developing an application without storing the source code in a version control system seems to be very silly nowadays.  
Tools like git, Mercurial, or Subversion are very useful to track code development, allowing branching for different features.

The term _codebase_ refers to the set of code repositories that refers to the system you are developing. 
A monolithic system must have only a code repository, while a distributed system (i.e. microservices) must have multiple code repositories, each of them that satisfy 12-factors.

For each code repository, it could be different running instance of the application (_deploy_) in different environments.

### 2. Explicitly declare and isolate dependencies
As we know. most programming languages offer a packaging system for distributing support libraries, such as npm for node.js, Rubygems for Ruby or Maven for Java.
Libraries installed through a packaging system can be installed system-wide  or scoped into the directory containing the app.

In order to replicate same behaviour multiple times in multiple enviornment, a best practice is to never relies on implicit existence of system-wide packages, nor to any system tool (i.e `curl`).
So your code repo must contains a _dependency declaration manifest_ (`package.json` for Node.js, `pom.xml` for Maven-build Java) that declares all dependencies, completely and exactly.

### 3. Store config in the environment
As we know, each application has a configuration, that could be different among enviroments.
The configuration could contains credentials for external services, feature toggles, etc.

There are a couple of approaches about storing configuration:
* using config file not checked into revision control systems
* using environment variables

### 4. Treat backing services as attached resources
As reported in the site,
> A backing service is any service the app consumes over the network as part of its normal operation. Examples include datastores (such as MySQL or CouchDB), messaging/queueing systems (such as RabbitMQ or Beanstalkd), SMTP services for outbound email (such as Postfix), and caching systems (such as Memcached).

The best practice here is to manage all backing services as external resources, without distinctions, accessing them via credentials and/or URL stored in configuration.

### 5. Strictly separate build and run stages
This is the most obvious and older of the best practice.  
Except from development deploy, to transform a codebase in a deploy through three stages:
* _build_, where the code will be converted in a executable bundle that contains all the dependencies;
* _release_, that takes the bundle and combines it with current confgiguration for the enviroment;
* _run_, that execute the release in a specific environment.

These stages must be isolated from each other.

### 6. Execute the app as one or more stateless processes
Each application runs in the enviroment as one or more operating system processes.
These processes must be stateless; any data that needs to persist must be stored in a stateful backing service.

### 7. Export services via port binding
If your application is a web application or need to expose something on the Net, you do not need to rely on runtime injection of a webserver into the execution environment to create a web-facing service.
The web app exports HTTP as a service by binding to a port, and listening to requests coming in on that port.

This is typically implemented by using dependency declaration to add a webserver library to the app, like `express` for Node.js or `Jetty` for Java.

### 8. Scale out via the process mode
As said before, every application runs as one or more processes in the operating system.
Sometimes the load that your application needs to manage increase rapidly, so you need to scale your application.  
Translating this into proceses model, to scale means to vary the number of running processes, of different types if you need to manage various workloads.

This means that you need to consider processes as _first class citizen_, as described in Unix processes model, and you should leverage operating system's process manager to manage output streams, respond to crashed processes, and handle user-initiated restarts and shutdowns.

### 9. Maximize robustness with fast startup and graceful shutdown
The process where the application runs must be _disposable_, so it could start and stop on-demand.
This facilitates fast elastic scaling, rapid deployment of code or config changes, and robustness of production deploys.

To do that, you need to minimize startup time and manage shut down gracefully when they receive a SIGTERM signal from the process manager.
In addition, you must develop a robust application against unexpected deaths.

### 10. Keep development, staging, and production as similar as possible
This factor seems to be the more obvious, but became more valid as Continuos Integration and Continuous Deployment methodologies were adopted.

As reported in the site,
>Historically, there have been substantial gaps between development (a developer making live edits to a local deploy of the app) and production (a running deploy of the app accessed by end users). These gaps manifest in three areas:
> * The time gap: A developer may work on code that takes days, weeks, or even months to go into production.
> * The personnel gap: Developers write code, ops engineers deploy it.
> * The tools gap: Developers may be using a stack like Nginx, SQLite, and OS X, while the production deploy uses Apache, MySQL, and Linux.

So you need to minimize all these gaps to encourage to deplo continuously and quicly.

### 11. Treat logs as event streams
I already talked about the importance of logs in enterprise application, and this an old fashioned best practice: logs allows to understand what is happening in your app running in a specific environment.

If in the past you use to save your logs in files, currently it is no more a best practice.  
So, writing your logs in the output stream of your process and evenutally collecting in an external storage it would be more feasible.

### 12. Run admin/management tasks as one-off processes
Instead of running administative and management processes from the application you are running, the best practice is to define a set of one-off processes that shoud run on-demand and execute them in same way as application processes (dependency isolation).
