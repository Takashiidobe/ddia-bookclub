# Chapter 1 Questions

## What does it mean for most applications to be data-intensive instead of compute-intensive?

CPU power isn't the main bottleneck -- the bottleneck is more due to the sheer
size and amount of data itself rather than any operations that are done over
the data.

## What is the definition of a reliable system? What are some of the ways that systems can become "unreliable"?

A secure system, tolerant of crashes and errors, that will consistently perform
its required use case. A system can become unreliable from faults or errors
from humans, software or hardware. A system can also be unreliable due to high
load and volume, causing it to not behave in a way we'd expect.

## What makes a system scalable? What are the characteristics of a scalable system?

A system is scalable if it's able to cope with increased load (to an extent)
gracefully by simply throwing more resources at it. A scalable system is
usually: non-monolithic (broken up into services with distinct responsibilities
that are easy to scale themselves); sharded (data is sharded and handled by a
subset of the system, rather than shared across the entire system); and
adaptable (easy to modify, either by adding more tasks or adding an new
service).

## What is the difference between a horizontally scaling and vertically scaling system? What are some of the pros and cons for each?

Vertically scaling is just making the machines larger (more CPU, more memory).
Horizontal scaling is adding more tasks so the load is distributed. Vertically
scaling is simpler, more expensive, and sometimes needed (caches). Horizontally
scaling is more complex and flexible (spinning up more tasks when load is high,
and turning down when load is low).

## What makes a system maintainable? What makes a system "unmaintainable"?

Operability, simplicity and extensibility make a system maintainable.
Documentation also makes a system more maintainable. A few things that can make
a system unmaintainable are: high operational overhead (lots of things require
manual input) and complex components that have many responsbilities (making it
hard to change, debug).

## Explain a project where you had to make it more reliable/scalable/maintainable. How did you do it, and what were some of the roadblocks in achieving the systems goals?

Project that attempted to come up with new metrics for throttling (to improve
reliability of our system). I had to come up with several potential metrics
which could be used to identify large, expensive and/or long-standing RPCs
(so we could abort them, or throttle the user's next queries). Involved lots
of plumbing and manual resource tracking of CPU, memory, walltime.

## Read three of the post-mortems in this [repo](https://github.com/danluu/post-mortems). What are some of the ways in which outages are caused? What could the teams have done to mitigate the blast radius or lowered the chance of such an outage happening? Explain the outage in your own words and give an explanation for what you learned from each summary.

1. https://azure.microsoft.com/en-us/blog/update-on-azure-storage-service-interruption/

Azure Storage Services became unavailable across several regions after a config
change caused tasks to crash loop. To mitigate the impact, config changes
should've been applied incrementally and slowly (so you can fully observe
the impacts of the change). Learnings: to be safe, you need to move slow
(to decrease blast radius). Changes must be canaried and qualified before
proceeding to the rest of the production.

2. https://status.cloud.google.com/incident/compute/15056#5719570367119360

Google Compute Engine instances permanently lost data when the datacenter was
struck by lightning four times. This data was stored only in one datacenter. To
improve durability of the data, we could have replicas sit in different
datacenters or regions in return for increased latency. Learnings: if you
really want durable data, you can't store all your data within a single region.
Replicas within a single datacenter are not enough, you need to have replicas
in other datacenters or regions too.

3. https://medium.com/xandr-tech/2013-09-17-outage-postmortem-586b19ae4307

AppNexus had a crashing bug (double free) that was not caught by their staging
environments. To mitigate this: make your staging environment as close as
possible to the production environment. And write better tests. Learnings: Even
for small changes, you should always write a test and make sure all code paths
are exercised. Staging environments are not as useful if they are not configured
exactly the same as production environments (and with the same data if
possible).

## Reflect on your personal experience. What are some of the ways that your team keeps services healthy and responsive? How do you test services? How do you react quickly to an unhealthy service?

Some ways we keep our services healthy and responsive:

- playbooks for operational tasks
- dashboards and metrics that track:
  - health of the system
  - health of consumer systems
  - health of systems we're dependent on
  - health and correctness of features
- self-healing -- try to have the system recover on its own
  - shedding load when hot
  - throttling bad users
  - re-sharding data when hotspots appear

Some ways we test services:
- large-scale staging and development environments that have tee'd traffic from
production.
- performance testing
- correctness testing

React quickly to unhealthy services by having the proper monitoring and
alerting set up, with well-written and comprehensive playbooks to follow.
