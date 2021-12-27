# Chapter 1 Questions

## What does it mean for most applications to be data-intensive instead of compute-intensive?

Managing the amount, complexity, and rate of change of data is more commonly the limiting factor for applications. Increasingly, application developers need to act as data system engineers and need to figure out which tools and approaches are most appropriate for the tasks at hand.


## What is the definition of a reliable system? What are some of the ways that systems can become "unreliable"?

A reliable system should *work correctly* (as the user expects, have good performance, prevent unauthorized usage). This needs to persist even in the event of certain *faults*, which could cause a system to become unreliable, e.g. the system is *fault-tolerant*. Faults could be hardware faults, software errors, or the result of human errors.


## What makes a system scalable? What are the characteristics of a scalable system?

A system can be said to be scalable if it can cope with an increased load described as *load parameters*. This can be measured by evaluating the difference in performance of a system as the load changes, or estimating how many resources need to be provided to a system for it to provide the same performance under an increased load. 


## What is the difference between a horizontally scaling and vertically scaling system? What are some of the pros and cons for each?

Vertical scaling (onto a more powerful machine) is often simpler than horizontal scaling (distributing the load onto multiple machines), but tends to be more expensive. Stateless systems can more easily be distributed while this is more complicated for stateful systems.


## What makes a system maintainable? What makes a system "unmaintainable"?

An operations team can help keep a system maintainable by monitoring and preserving system health. Managing system complexity by decoupling of modules, having consistent naming and terminology, and reducing special cases work arounds can also add to the maintability of a system. Agile approaches to system development also contribute. Ignoring these, often because of budget or scheduling, leads to an increase in complexity, poor system health, and to low system modifiability, making the system harder to maintain. 


## Explain a project where you had to make it more reliable/scalable/maintainable. How did you do it, and what were some of the roadblocks in achieving the systems goals?

Starting a job soon, will come back to this.


## Read three of the post-mortems in this [repo](https://github.com/danluu/post-mortems). What are some of the ways in which outages are caused? What could the teams have done to mitigate the blast radius or lowered the chance of such an outage happening? Explain the outage in your own words and give an explanation for what you learned from each summary.

#### Netflix
AWS experienced a service degradation. Once netflix noticed the issue, it identified that the degradation was limited to a single availability zone and enacted a zone evacuation away from the affected zone, restoring service. Netflix seems to be really on top of their game when it comes to desiging high availability, as they had practied zone evacuation and their system had much in-built redundancy.

#### Redit
During a migration, the package management system noticed a manual change and reverted it. The autoscaler, reading the partially migrated data, terminated many application servers. Risky migrations are risky, minmize their complexity by disabling auxillery systems that may behave unexpectedly during migrations. Too much fault tolerance can be a fault of its own.

#### Medium
People can't type "Ś" on the website medium. This is because the site disabled the functionality of CTRL-S (which serves to save the webpage) from medium's text editor. The right ALT key on windows internally maps to CTRL ALT. Right ALT+S actually mapped to CTRL+ALT+S, which medium disabled (by vistue of containing CTRL+S). There will always be issues that arise when the vastness of the human condition intersects with anything done by said humans. I suggest that we make our software easy to modify to quickly rectify such issues rather than trying address them all before. I also suggest medium to remember that they've changed the blocking of CTRL+S *only if ALT is not pressed*. This could also lead to a similar issue in the future.

The outages were caused by the failure of system (AWS) on which Netflix depended, an automated process intended to tolerate faults behaving in an unanticipated manner (Software/Human Error; Reddit), or a deliberate change which repercussions that were not anticipated (Human Error; Medium).


## Reflect on your personal experience. What are some of the ways that your team keeps services healthy and responsive? How do you test services? How do you react quickly to an unhealthy service?

Coming soon.™
