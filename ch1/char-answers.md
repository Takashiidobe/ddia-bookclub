# Chapter 1 Questions

## What does it mean for most applications to be data-intensive instead of compute-intensive?

data is its primary challenge—the quantity of data, the complexity of data, or the speed at
which  it  is  changing—as  opposed  to  compute-intensive,  where  CPU  cycles  are  the
bottleneck.

## What is the definition of a reliable system? What are some of the ways that systems can become "unreliable"?

The system should continue to work correctly (performing the correct function at
the desired level of performance) even in the face of adversity (hardware or soft‐
ware faults, and even human error).

## What makes a system scalable? What are the characteristics of a scalable system?

As the system grows (in data volume, traffic volume, or complexity), there should
be reasonable ways of dealing with that growth.

## What is the difference between a horizontally scaling and vertically scaling system? What are some of the pros and cons for each?

#Vertical is Increacing the power of each machine (RAM,CPU, etc).
Pro
Easy to do/simpler 
Con
Cost of increacingly more powerful hardware

#Horizontal is Increacing the number of machines.
Pro
Can use cheaper hardware.

Con
Way more complex

## What makes a system maintainable? What makes a system "unmaintainable"?

maintainable : Easy to keep running, for new people to onboard to developing and extensible enough for new features.
unmaintainable: Hard to keep service running, and difficult to extend features or onboard new devs.

## Explain a project where you had to make it more reliable/scalable/maintainable. How did you do it, and what were some of the roadblocks in achieving the systems goals?

I took a service from hackathon project to production.  This involved, converting it from a cronjob that ran each hour on a single box, to a containerized service that runs in a k8s cluster with a work queue to distribute the work horizontally.
This made it easier to run, by just applying yaml to a k8s cluster, more reliable in that the managed k8s service will manage my replicas and keep enough pods running and more scalable in that we didn't need the one box to finish its work within the hour.

## Read three of the post-mortems in this [repo](https://github.com/danluu/post-mortems). What are some of the ways in which outages are caused? What could the teams have done to mitigate the blast radius or lowered the chance of such an outage happening? Explain the outage in your own words and give an explanation for what you learned from each summary.

Outages are often times caused by human error.  They could be better prevented with more automated guardrails around ensuring that config changes are not only valid, but also safe to do/tested with canaries.
Additionally, services should be resilient to stampedes or failures in other services they interact with, to prevent cascading failures.
https://blog.cloudflare.com/todays-outage-post-mortem-82515/
Cloudflare pushed a bad config to all routers causing them to all fail.
Should test on only a subset of traffic at a time, to make sure things are good w/ canaries to rollback if needed or if anything only cause degredation not outage.

https://status.cloud.google.com/incident/compute/16007
A bad GCP config change was propagated.  Similarly, testing with only a subset would have helped ensure the config was good to go.

https://azure.microsoft.com/en-us/blog/update-on-azure-storage-service-interruption/
A config change to a subset of customers exposed a bug that was not caught during the testing.
Even if we deploy only to a subset of customers, we need good enough observability within those to determine if it is safe to propagate the deployment.

## Reflect on your personal experience. What are some of the ways that your team keeps services healthy and responsive? How do you test services? How do you react quickly to an unhealthy service?

1. Monitoring and Logging with Alarms.
2. Runbooks for those Alarms
3. Not deploying without testing

We test by having a few staged deployments, with development, testing, and production enviroments.  We do not deploy changes to all pods at once, but instead gradually replace pods with the updated version and can rollback if needed.
If something is degragated, or failing we have alarms that will fire for the incident to inform our oncall (who can contact others on the team) to investigate via a page.
The alarm will have a link to a runbook with steps to diagnose and possible reccomendations to remediate the issue.

