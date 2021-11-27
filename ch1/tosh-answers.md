# Chapter 1 Questions

## What does it mean for most applications to be data-intensive instead of compute-intensive?

Most applications nowadays are limited by the data they work with. Such as how much data an application uses, how complex the data is and the speed at which the data changes. If the application is bound by those attributes in any way then the application is considered data-intensive. If an application is bottlenecked by cpu processing power and is constantly being held back by processing power it would be considered computer-intensive.

## What is the definition of a reliable system? What are some of the ways that systems can become "unreliable"?

A reliable system is a system that continues to work correctly even in the face of adversity(hardware/software faults, human error). A system can become unreliable when it has faults that can lead to failure of the system. A few common causes of system failure that lead to unreliable systems are hardware faults(ex. failing disks), software errors(ex. software with bugs) and human errors.

## What makes a system scalable? What are the characteristics of a scalable system?

A scalable system is a system that can grow and have reasonable ways of dealing with that growth. Characteristics of a scalable system are being able to handle increased load(i.e increased writes and reads). Two often used ways of describing scalable systems are horizontal scaling(scaling out across more machines) and vertical scaling(scaling up to use more powerful machines). For a system to scale properly it needs to have ways of describing load and performance quantitatively so it can when/why it needs to scale.

## What is the difference between a horizontally scaling and vertically scaling system? What are some of the pros and cons for each?

Horizontal scaling is when a system can spread its load across a large number of smaller machines by scaling out. Vertical scaling is when a system can scale up to meet the load by using more powerful machines. It is often best to use vertical scaling when the system can still fit on a single node, and then to upgrade to a distributed(horizontal) system when the scaling cost or availability requirements force a system to become distributed. 

Vertical scaling pros
- easy initially
- less complex

vertical cons
- expensive machines
- less elastic

Horizontal pros
- easy to add additional resources
- cheaper machines

Horizontal cons
- can become very complex scaling from single node to multiple nodes


## What makes a system maintainable? What makes a system "unmaintainable"?

A system is maintainable if it has operability(a system should be easy for an operations team to keep running smoothly), simplicity(should be simple for new engineers to understand the system and as much complexity should be removed as possible) and evolvability(engineers can easily make changes to the system in the future and adapt to unanticipated use cases as requirements change). A system is unmaintainable if it is difficult for people to work on, understand or make changes to.

## Explain a project where you had to make it more reliable/scalable/maintainable. How did you do it, and what were some of the roadblocks in achieving the systems goals?

On one of my personal projects I was building an image recognition service that needed to be callable by an existing application. Users would interact with this new system through a Discord bot meaning that the program had to be both reliable and scalable so that the system was always online and users would have fast responses. I was able to accomplish these requirements by rewriting some of the applications architecture so the new computationally expensive feature on the bot by 1) making it so the new feature did not block less computationally intensive features on the bot(i.e. async) and 2) reducing the runtime of the image recognition so that the responses were under < 10s allowing computing resources to be used on other tasks. Some of the roadblocks in achieving these systems goals were learning how the original system worked even though it was undocumented, and also doing some profiling on the image recognition so I could find what was causing the longer runtime. 

## Read three of the post-mortems in this [repo](https://github.com/danluu/post-mortems). What are some of the ways in which outages are caused? What could the teams have done to mitigate the blast radius or lowered the chance of such an outage happening? Explain the outage in your own words and give an explanation for what you learned from each summary.

1. https://blog.cloudflare.com/cloudflare-outage-on-july-17-2020/
An error in cloudflare's backbone network config caused almost 50% of their network to go down. This error specifically happened when a new config was pushed to the backbone that redirected a large portion of the backbone traffic to one location. This large amount of traffic quickly overwhelmed that data center causing outages throughout their network. One way to mitigate these errors would have been to change the preference for local servers on the BGP network so a single data center cannot attract all of the traffic on the network. 

2. https://github.blog/2016-02-03-january-28th-incident-report/
A power disruption to a github server farm caused an interruption in service. When power was restored most of the services did not come back online as expected. It was discovered that the servers were stuck in reboot cycles due to dependencies on a redis cluster that failed to come back online after the power outage. A closer look at the redis server showed that it was unable to come online because of a firmware bug where the hard drives were not available after a reboot. One way to mitigate the impact of an event like this in the future would be to have power reserves for critical infrastructure like the redis cluster that all the other services were dependent on. Additionally keeping all firmware as up to date as possible will minimize the chances that bugs will remain on hardware that is in use.

3. https://lkml.org/lkml/2012/7/1/203

A bug in the linux kernel caused leap seconds to be improperly implemented. When a leap second was set it caused a 1 second mismatch between CLOCK_REALTIME and TIMER_ABSTIME. This meant that whenever a call was made to sleep that was less than a second then an interrupt would be called immediately without waiting/sleeping the proper amount of time. This lead to any code that had loops with sleep calls having load spikes due to the immediate return of the sleep calls causing application to use a large amount of processing power until clock_was_set was called. Because of the nature of this bug it would be very hard to mitigate the blast radius in any meaningful way. The best way to ensure that this doesnt happen in the future would be to place less trust in the OS that programs are running on and verify behavior of OS functionality. 


## Reflect on your personal experience. What are some of the ways that your team keeps services healthy and responsive? How do you test services? How do you react quickly to an unhealthy service?

As a relatively new engineer I dont have a lot of experience keeping services healthy and responsive, however within this time I have experienced how to test services to ensure that most behavior is verified before it ever reaches a customer. One way that I have done this is to run end to end tests, Unit tests, and integration tests on services to see how they would behave in the many scenarios that a customer might get the application into. I also dont have any experience reacting to unhealthy services, but I imagine if an unhealthy service were to occur my number one priority would be to figure out what is causing the service to be unhealthy as quickly as possible so a fix could be pushed out ASAP.