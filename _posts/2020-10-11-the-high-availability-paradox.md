---
layout: post
title: The High Availability paradox
categories:
  - Best_practices
  - Cloud_development
---

We are continuously looking for an application always available.
More our application will be available, more we could get new customers and increase our revenue.   
But an application _cannot_ be _always_ available.

As we know, if you perform an update adding new features or fixing bug, you will cause an outage because old TCP connections to your server will be closed and no new connections will be accepted until the server will be up and running again.
This is why we always look for **high availability**.  
But also high availability is a paradox, because the same maintenance operations are mandatory.
Even if you could accept few seconds, minutes or hours of downtime, you need to arrange your continuous delivery scripts in order to reduce downtime (e.g. using blue-green deployments and redirecting traffic like in A/B testing).

You could test everything before a deploy, be sure your deploy script will not cause outages, but you cannot forecast everything and an unexpected event, like a network disruption or a power outage in your datacenter. could happen.
So looking for an high available application is uthopic: you could hope so, you could work to achieve that, but you could not control everything.