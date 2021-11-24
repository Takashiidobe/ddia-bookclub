# Chapter 1 Questions

## What does it mean for most applications to be data-intensive instead of compute-intensive?

Most applications nowadays are limited by the data they work with. Such as how much data an application uses, how complex the data is and the speed at which the data changes. If the application is bound by those attributes in any way then the application is considered data-intensive. If an application is bottlenecked by cpu processing power and is constantly being held back by processing power it would be considered computer-intensive.

## What is the definition of a reliable system? What are some of the ways that systems can become "unreliable"?

A reliable system is a system that continues to work correctly even in the face of adversity(hardware/software faults, human error). A system can become unreliable when it has faults that can lead to failure of the system. a few common causes of system failure that lead to unreliable systems are hardware faults(ex. failing disks), software errors(ex. software with bugs) and human errors.

## What makes a system scalable? What are the characteristics of a scalable system?

A scalable system is a system that can grow and have reasonable ways of dealing with that growth. Characteristics of a scalable system are being able to handle increased load(i.e increased writes and reads). Two often used ways of describing scalable systems are horizontal scaling(scaling out across more machines) and vertical scaling(scaling up to use more powerful machines). For a system to scale properly it needs to have ways of describing load and performance quantitatively.

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

On one of my personal projects I was building an image recognition application that needed to be callable by an existing application. Users would interact with this new system through a Discord bot meaning that the program had to be both reliable and scalable so that the system was always online and users could have fast responses when using the new feature. I was able to accomplish these requirements by rewriting some of the applications architecture so the new computationally expensive feature on the bot did not block other less intensive features on the bot, while also reducing the runtime of the image recognition so that the image recognition responses were taking < 10s. Some of the roadblocks in achieving these systems goals were learning how the original system worked even though it was undocumented, and also doing some profiling on the image recognition so I could find the what was causing the longer runtime. 

## Read three of the post-mortems in this [repo](https://github.com/danluu/post-mortems). What are some of the ways in which outages are caused? What could the teams have done to mitigate the blast radius or lowered the chance of such an outage happening? Explain the outage in your own words and give an explanation for what you learned from each summary.

An error in a cloudflare config change caused their backbone network to go down
A power disruption to a github server farm caused an interruption in service that was then prolonged by errors while the systems came back online

## Reflect on your personal experience. What are some of the ways that your team keeps services healthy and responsive? How do you test services? How do you react quickly to an unhealthy service?

-- YOUR ANSWER HERE --
