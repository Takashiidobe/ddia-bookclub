# Chapter 1 Questions

## What does it mean for most applications to be data-intensive instead of compute-intensive?
- Compute intensive applications rely on the power of horizontal scaling and the use of CPUs. Compute intensive applications will scale via adding more CPUs aka more compute power. A data intensive application relies on common modules used for handing data. With data driven applications we rely on specific common tech, where each building block has some specific use. Data driven applications ushered in the modern microservices architecture.

### Some of the common Modules Mentioned
- Databases used for persistence of data
- Caches used for fast fetching of data, often used to save results of a compute heavy task, and to speed up subsequent calculations.
- Search Indexes, used to quickly search or filter through a large quantity of data.
- Stream Processing, which can be used to offload work and queue it onto other machines. We can scale the workload to other machines and solve problems in an async fashion.
- Batch Processing. Batch processing is a CPU intensive task in which we crunch batches of information/data in batches to lessen the given workload. Often is the individual action or task triggered by teh Stream processing layer, or some queue to ensure successful computation.

## What is the definition of a reliable system? What are some of the ways that systems can become "unreliable"?

- A system is considered reliable so long as it has no faults. It needs to function even in the case of adversity. Netflix engineering has a process called chaos engineering used to test this in their systems. They will shutdown and wreak havoc on their systems making sure that there is no world where there backups fail. They cut off databases, power to their data centers, access to critical nameservers. 

- An example of an unreliable system would be a system that relies on one database, or many databases in the same location. A system is unreliable if its critial points of failure dont have a backup system in place. An unreliable system will crash on wacky user input. An unreliable system is susceptible to human error.

- A reliable system will havve proper logging and exceptions so that worst case we see a bug or error in our application we have a way to immeadiately track it. test throughly and all of your edge cases to avoid that bad input in which can bring down your system. sheesh. Set up backups and ways for quick and easy recovery. 

## What makes a system scalable? What are the characteristics of a scalable system?

- As our system grows in size there should be a reasonalbe way to deal with that growth that doesnt involve input from the maintainer. It should be responsible for provisioning and scaling itself at each layer of the application. If one shard of data will be read from the most we provision celebrity sharding. If we forsee that one expensive task will be recomputed 100s of times we implement caching. A scalable system is one that with a higher volume of requests, traffic or strain, it stays reliable.

## What is the difference between a horizontally scaling and vertically scaling system? What are some of the pros and cons for each?
- To answer this question we must first answer another set of questions

### What is Horizontal Scaling?
- Horizontal scaling refers adding new nodes aka machines to your infra to cope with new load. Its often favored to vertical scaling due to parelellization of work. 
### What is Vertical Scaling?
- Verticially scaling refers to adding additional resources to your machines so that it meets your demands. Improve the memory, disk space, CPU core.

### Pros of Scaling Horizontally
- Scalling is easier from a hardware perspective. No need to upgrade old machines, just provision new ones.
- Fewer periods of outages.  You won't be concerted with switching off an old machine right away. You won't have to stop the current process to scale. Just add a new machine.
- Increased Fault Tolerance. If you move your data to a separate machine you will no longer rely on that single point of failure. In case of failure we can rebalance the load. 

### Cons of Scaling Horizontally
- Increased maintenance. Its much harder to maintain the health of a group of servers, that could be spread all accross earth. You have to add software for load-balancing as well as backing up all of your machines. Also if your system relys heavily on stream processing we will require more complex software to insure nodes stay synchronous.
- More expensive. Just buying new servers to fix a problem cost a lot up front. Servers are expensive.

### Pros of Scaling Vertically
- Less Complicated process of communication. When one node is responsible for all levels of the system, its all on the one device, we dont have to concern our selves with communication with other machines. This will result in faster response times.
- Maintenance is less complicated. it is less complex because the number of nodes you manage is less.
- Less need for software changes. Once you have configured a given server, there isnt a lot of need to update the software to communicate with new devices. 

### Cons of scaling Vertically
- Increase in risk of failure. Having all of your operations in one server/device means that you have a critical point of failure that if that machine dies you risk losing data. - Hardware limitations. There is only so much you can upgrade a given machine. You will reach a wall at some point. 
- Increase in amount of downtime. Whenever we upgrade that machine we will have to take it offline.  

## What makes a system maintainable? What makes a system "unmaintainable"?

-- YOUR ANSWER HERE --

## Explain a project where you had to make it more reliable/scalable/maintainable. How did you do it, and what were some of the roadblocks in achieving the systems goals?

-- YOUR ANSWER HERE --

## Read three of the post-mortems in this [repo](https://github.com/danluu/post-mortems). What are some of the ways in which outages are caused? What could the teams have done to mitigate the blast radius or lowered the chance of such an outage happening? Explain the outage in your own words and give an explanation for what you learned from each summary.

-- YOUR ANSWER HERE --

## Reflect on your personal experience. What are some of the ways that your team keeps services healthy and responsive? How do you test services? How do you react quickly to an unhealthy service?

-- YOUR ANSWER HERE --
