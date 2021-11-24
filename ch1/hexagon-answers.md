# Chapter 1 Questions

## What does it mean for most applications to be data-intensive instead of compute-intensive?

Data-intenstive implies that most of the engineering challenges revolve around the data: whether that be the size, complexity, or the speed that the data changes. On the other hand, compute-intensive applications have CPU power as their main bottleneck. 

## What is the definition of a reliable system? What are some of the ways that systems can become "unreliable"?

A reliable system is a system that continues to work correctly under stress and adversity. A system can become unreliable if it fails to address hardware faults, software errors, or human/user error.

## What makes a system scalable? What are the characteristics of a scalable system?

A system is scalable if it is able to cope with increased load. The characteristics of a scalable system will heavily depend on which "load parameters" are the most important. But in general, a scalable system should maintain acceptable performance as we increase some load parameters. In order to actually see these relationships, scalable systems should be observable (logs / metrics / dashboarding).

## What is the difference between a horizontally scaling and vertically scaling system? What are some of the pros and cons for each?

Vertically scaling a system involves directly upgrading to a bigger machine. On the other hand, horizontally scaling distributes the load to multiple (usually smaller) nodes that communicate via a network.

Vertical scaling is conceptually simple and doesn't require much modification to the software. However, it's expensive and machines are often constrained by physical limits.

Horizontally scaling is cheaper, but it's often more complex. Scaling horizontally often demands understanding of concepts like networking and load balancing which are not trivial to reason about. 

## What makes a system maintainable? What makes a system "unmaintainable"?

In short, a maintainable system is one which all of engineers / operations can work with productively. There are 3 main traits we look for in a maintainable system: Operability, simplicity, and evolvability. If we lack any of these, the system is "unmaintainable". 

Operability means that the operations team can easily work with the system. Without clear processes for operations, the ops team will struggle to keep the system running smoothly. 

Simplicity means new engineers can easily learn and reason about the system. As systems become larger and more complex, they often become difficult to understand and reason about. Abstractions should be used to hide complexity. 

Evolvability is the idea that engineers should be able to easily change the system to adapt to new requirements. Software systems may need to change at any moment due to unanticipated use cases, changing business requirements, feature requests from users, etc. It's important for engineers to easily adapt the system to these changes. 

## Explain a project where you had to make it more reliable/scalable/maintainable. How did you do it, and what were some of the roadblocks in achieving the systems goals?

I made a legacy system more maintainable by revamping the backend to use more modern technologies. The system was old af and using technologies that weren't well supported, which made it hard to reason about.

## Read three of the post-mortems in this [repo](https://github.com/danluu/post-mortems). What are some of the ways in which outages are caused? What could the teams have done to mitigate the blast radius or lowered the chance of such an outage happening? Explain the outage in your own words and give an explanation for what you learned from each summary.

The outages are caused by a variety of factors including human error, unexpected growth, and malicious actors. I think all of these teams could have benefited from establishing clear and automated processes earlier to deal with these issues as quickly as possible. 

https://dougseven.com/2014/04/17/knightmare-a-devops-cautionary-tale/

A big trading firm botched a manual deployment, and lost $460 million in less than an hour. I think this highlights the importance of operability, as this could have been mitigated with better processes for deployment and troubleshooting. 

https://www.epicgames.com/fortnite/en-US/news/postmortem-of-service-outage-at-3-4m-ccu

Epic games experienced a peak of 3.4 million concurrent users, which caused disruption. I don't think it's reasonable to have expected such fast growth, so I think it's important to make the system elastic and adaptable to unexpected load.

https://github.blog/2018-03-01-ddos-incident-report/

Github got ddos'd. This shows the importance of considering security during the design of your system early to proactively defend against attacks like this.

## Reflect on your personal experience. What are some of the ways that your team keeps services healthy and responsive? How do you test services? How do you react quickly to an unhealthy service?

To keep services healthy, my team has a pretty robust observability system with logs, metrics, dashboarding, and alerts to catch issues before they cause major impact. We also incorporate redundancy, with machines in multiple geographic locations if something goes wrong. 

During development we test services via unit / integration testing. We also will deploy in multiple stages prior to prod so we can catch issues before they cause user impact.  

If a service is unhealthy, we receive an alert. A designated person will investigate the issue ASAP to stop any bleeding. Then we typically follow up with a further investigation to find the root cause.
