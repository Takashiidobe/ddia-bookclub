# Chapter 1 Questions

## What does it mean for most applications to be data-intensive instead of compute-intensive?

Data intensive applications have memory / storage operations as a bottleneck whereas compute intensive applications have bottlenecks around processing and computation.

## What is the definition of a reliable system? What are some of the ways that systems can become "unreliable"?

A reliable system is like a good friend or network of friends you can count on to do the right thing even when times get tough (your car broke down at 3am or you vomited on the carpet in their 4k/mo apartment in NYC). Systems are unreliable if they can't produce the correct results (drive you to your old address) or fail to meet performance requirements (don't answer your drunk texts at 4am).

There are three main types of failures that a reliable system must handle: hardware failures, software failures, and human errors. A system becomes 'unreliable' by failing to deal with one or more of these error types.

For hardware, this means not having enough redundancy (e.g. enough friends to call on you). For software, there are many ways a failure can impact and propagate through a system. A system can become unreliable if we don't have enough monitors in production to detect possible software issues or automated tests to ensure the correct behavior. It can become unreliable if we're not failing gracefully when we run into issues like when your external api requests are getting rejected and you end up drunk dialing your ex.

For human errors, a system becomes unreliable when we're not considering how dumb humans can be and fail to design our software to prevent dumb humans from doing dumb human things with respect to configuration, especially. When you lack telemetry so you can't know when your friends are in the capital on Jan 6th and you can't understand why half of your friends are antivaxxers. When you stop adding tests to verify that the changes you make to the system aren't going to brick your jailbroken Linux PS5. When you have no way to undo your mistakes (you can't rollback text messages unfortunately). When management cares more about delivering feature X than upholding existing SLAs.

## What makes a system scalable? What are the characteristics of a scalable system?

Scalability is how much a system can COPE with increased load. A scalable system must handle multiple dimensions from increased reads and writes to improving service quality (meeting stricter SLAs), expanding service to multiple regions, and adding redundancy. A scalable system must allow for the improvement of existing components and the addition of new components to meet new performance goals.

## What is the difference between a horizontally scaling and vertically scaling system? What are some of the pros and cons for each?

### Vertical Scaling
Scaling up or adding resources to one machine/server.

#### Pros
Less complex

#### Cons
Exponentially more expensive to keep improving one machine
Upgrades to a single machine are limited

### Horizontal Scaling
Scaling out or adding more machines/servers.

#### Pros
More cost effective

#### Cons
More complications and failure modes

## What makes a system maintainable? What makes a system "unmaintainable"?

A maintainable system is one that can be kept operating in its current capacity as well as extended to support other use cases. Over time, the people operating, maintaining, and building upon the system will change so this work must be manageable for new staff. So a system becomes unmaintainable or degraded when one of these properties is lost. This can occur gradually from a build up bugs and technical debt (lack of automation, lack of test coverage, lack of documentation, etc.). Lack of automation wastes manhours maintaining and supporting manual processes. Lack of test coverage contributes to a build up of bugs, undetected regressions. Lack of documentation, especially for troubleshooting, wastes manhours on resolving the same issues and reinventing the wheel.

A system is not only the software that comprises the service/platform and the hardware the software runs on, but also the people and the organizations that support it. Knowledge sharing is a critical component of maintainability. Concentrating information in a handful of specialists means that expertise will be lost when they are no longer around to support the system. Usually maintainability is lost when an organization doesn't prioritize it. This has downstream effects that reduce reliability and scalability as maintainenance issues start consuming more and more capacity.

## Explain a project where you had to make it more reliable/scalable/maintainable. How did you do it, and what were some of the roadblocks in achieving the systems goals?

Built a microservice to act as a control layer for cloud storage. It had two components, one that managed the storage and retrieval of blobs in Azure Blob Storage and the other managed the creation of storage accounts. Azure Blob Storage allows for unlimited storage in each storage account, but places hard limits on bandwidth for each storage account. Our use case was both read and write heavy, so in order to handle increased loads on both we needed a way to scale our bandwidth by adding storage accounts.

We needed to a method of ensuring that blob requests are routed to the same storage account they were originally written to. We hashed the storage account name and embedded it in the blob ids. We also needed support existing records, so we had to add handling for the legacy storage account, which did not have the modified blob ids.

## Read three of the post-mortems in this [repo](https://github.com/danluu/post-mortems). What are some of the ways in which outages are caused? What could the teams have done to mitigate the blast radius or lowered the chance of such an outage happening? Explain the outage in your own words and give an explanation for what you learned from each summary.

<<<<<<< HEAD
(Outage 1)[https://blog.google/inside-google/company-announcements/todays-outage-for-several-google/]
=======
[Outage 1](https://blog.google/inside-google/company-announcements/todays-outage-for-several-google/)
>>>>>>> f58b89a0314760d8be995ef74be3b45e18e088d5
Google had a bad autogenerated config due to a software bug, which took down most of its services for half an hour (to an hour for some users). The issue was detected about 7 minutes after the time of incident. Issue resolved itself after automation generated and started deploying a valid config.

Main takeaways in the doc prioritized fix of software bug (as well as audit of related code), additional validations needed to detected invalid config, additional monitoring needed to improve time to detection. Some other ways to mitigate the impact of this issue could involve testing config against a subset of services and rolling back configs when a config change triggers monitors.

<<<<<<< HEAD
(Outage 2)[https://azure.microsoft.com/en-us/blog/summary-of-windows-azure-service-disruption-on-feb-29th-2012/]
=======
[Outage 2](https://azure.microsoft.com/en-us/blog/summary-of-windows-azure-service-disruption-on-feb-29th-2012/)
>>>>>>> f58b89a0314760d8be995ef74be3b45e18e088d5
Microsoft Azure had an initial service outage caused by faulty datetime logic that generated a certificate by simply incrementing the date by one year. This logic failed on leap day since it tries to create a certificate with an invalid date. A secondary outage was caused when a quick fix was deployed to seven clusters which were impacted by the bug mid update. This fix actually broke networking across the cluster for VMs with older agents.

Several takeaways, but the key points of Prevention, Detection, Response, and Recovery are a great baseline for identifying repair items to act on.

Datetime libraries and code are prone to errors and edgecases. When operating at a massive scale, it's important to consider rare failure conditions and avoid assumptions about correctness and behavior. It's important to have this knowledge centralized to reduce redundant problem solving and ensure consistency across the organization.

Time to detection was 75 minutes after the first timeout, which would be greatly improved by classifying the timeouts as failures earlier. Transparency for the customer is important. They need to know that we are aware of the issue and are working to resolve it. Not only do customers need to be informed about service health, but they also need that information presented in a compact, user friendly manner.

<<<<<<< HEAD
(Outage 3)[https://blog.cloudflare.com/a-byzantine-failure-in-the-real-world/]
=======
[Outage 3](https://blog.cloudflare.com/a-byzantine-failure-in-the-real-world/)
>>>>>>> f58b89a0314760d8be995ef74be3b45e18e088d5
Cloudflare had an outage due to faulty network switch which recovered, but triggered a Byzantine fault (technically an omission fault in this case) in which their main strongly consistent key-value store entered a readonly state. The readonly state was triggered due to a Byzantine fault in the nodes following the RAFT protocol where one node couldn't communicate with the leader and kept initiating leader elections while the other nodes kept electing a leader that the first node couldn't communicate with. Two clusters were unable to write to the key-value store, which prevented them from reporting that they had healthy primary databases, which triggered automatic promotion of replicas, which also triggered rebuilds on all replicas due to a bug. Due to the built in redundancy, no system completely failed but rather experienced degraded performance.

Main takeaways are that the bug should be resolved and that there should be more redundancy in the system. There should be more levers for mitigation and, perhaps, drilling around failure scenarios. Six hours appears to be a rather long time for service disruption even if it is just degraded performance.

## Reflect on your personal experience. What are some of the ways that your team keeps services healthy and responsive? How do you test services? How do you react quickly to an unhealthy service?

We keep an on-call rotation to support our services, hold regularly meetings to triage bugs and learn from major incidents. We use a combination of automated and manual tests for our services. We keep a subset of the production environment consisting of whitelisted test machines and follow a nightly deployment process into the test in production machines. A weekly manual test pass by third party contractors in BVT machines gates the deployment train into the rest of the production environments. On-call engineers are expected to respond to low-priority incidents during working hours and customer impacting issues within a specific time to respond window defined by our SLA.
