# Chapter 1 Questions

## What does it mean for most applications to be data-intensive instead of compute-intensive?

It means the application's speed is not bottlenecked by the CPU but instead by their I/O speed i.e. the speed at which they can read and write data.

## What is the definition of a reliable system? What are some of the ways that systems can become "unreliable"?

A reliable system is one which is tolerant of various faults. While it's not possible to be resistant to all faults to due a need for both omniscience and omnipotence. It is still important to mitigate factors. A system can be unreliable due to being at great risk of hardware, software, and human error.

For instance, hardware faults are largily independant for each other and can be greatily mititgated with the proper redundancies at each level of operation where they appear. While software faults can by systematic. One issue can quickly cascade down to other layers. A way of tackling these issues to think carefully about assumptions you've made and how to handle exceptions when they occur. Finally there are human errors, they are much harder to to mitigate requiring designs decisions, testing, monitoring, and proper training. 

## What makes a system scalable? What are the characteristics of a scalable system?

A system is scalable if it is able to accomadatte an increase in its load. While still maintaining its preformance goals. The characteristics of a scalable system is one whose architecture is reflective of the needs of the system. Does it use more reads then writes? What are the size of the reads and writes? What are the common operations? What are the rare operations?

## What is the difference between a horizontally scaling and vertically scaling system? What are some of the pros and cons for each?

Vertical scaling is where you improve the machine so that i.e. a faster server. While a horizontally scaling server is one where you add machines and distrubte the workload. 
The pro of vertical scaling is that it is often simpler and easier to run, while the downside is that it is often more expensive, and a more powerful machine may not exist yet. Also there's the question of what do you do when that machine malfunctions?
Likewise the pro of a horizontally scaling system is that there is redundancy, if one of them fails, the system can still run albeit slower, and there is also cost. While for cons, it adds a layer of complexity to your system.

## What makes a system maintainable? What makes a system "unmaintainable"?

A system is maintainable if it satisifes 3 criteria, operability, simplicity, and evolvability. Operability means the system is built with the operation team in mind. Allowing them to easily operate, make changes, perform maintenance, and pull useful metrics. Simplicity, means being able to understand, and use. Code that is complex carry a burden of more frequent edge cases, and more time spent trying to understand. A good system should use abstractions to simplify. Evolvability is the ability to easily make changes to the system. A system needs change as time goes on, and a system that makes it costly is gonna be inrcedibly taxing time wise for the engineer. As well as encouraging poorly cobbled together bandaids.

## Explain a project where you had to make it more reliable/scalable/maintainable. How did you do it, and what were some of the roadblocks in achieving the systems goals?

I cobbled together my projects using the finest spaghetti code there is. But if I had to go back and re do things. I would abstract more heavily.

I already had abstractions, but when I went back I realize it was still difficult to parse what I had done previously. As well as adding needless complexity at times.

## Read three of the post-mortems in this [repo](https://github.com/danluu/post-mortems). What are some of the ways in which outages are caused? What could the teams have done to mitigate the blast radius or lowered the chance of such an outage happening? Explain the outage in your own words and give an explanation for what you learned from each summary.

>Configuration changes to Facebook's backbone routers caused a global outage of all Facebook properties and internal tools.
systematic 

This outage was caused by a issued command which took down all connections to facebook's backbone network. there was a bug with the audit system, which was suppose to prevent such a command from being issued. This combined with facebook's internal tools breaking due the outage caused facebook to be unable to resolve this issue. They were forced to send engineers directly to the data centers, which was time intensive to the phyiscal and system security. The team could have made their internal tools more redundant so that they could fix the issue. It's sort of like storing a lockpick in case you lose a set of keys, and keeping it behind a door. What happens if you lose the key to the door.

What I learned is that it's important to have redundencies for your internal tools. So that if one or several things fail, you can still recover, and reverse the damage. 

>Bad weather caused power failures throughout AWS US East. A single backup generator failed to deliver stable power when power switched over to backup and the generator was loaded. This is despite having passed a load tests two months earlier, and passing weekly power-on tests.

From what it sounds like despite regular testing and checks, when they were needed the both generators malfunctioned, and had to be manually brought online, causing an outage. The issue was that the voltage output of the generators was not stable and required onsite teams to stabalize. Based on the reading it appears amazon had measures in place. First with the backup generators in place in case they lost power as well as frequent testing of teh generators to ensure they work as intended.

Steps that could've been taking the reduce the chance and lower outage time would've been as stated to increase the size on the onsite engineering team so that they could've stabalized the generator sooner. As well as giving the generator more time to reach stable voltage before the need to switch over to them. 

>How HealthCare.gov Went So, So Wrong

What went wrong seems to be both incompetency and complexity. People who did not understand what requests were hard and what were easy, were in charge of making them. This caused requests to be made that were incredibly difficult. On top of that things were expected to work day 1, instead of iterating on smaller designs. The process in making the system was given to contracts in a manner full of bureacracy. It also encouraged contractors to do the minimum amount of work to get the job done.  

The government could've consulted with people knowledgable with building complex systems and gave them more authority to ditacte the scope and direction of the systems. They could have started small and iterated into the final product they wanted, instead of expecting the final product immediatily.  To be frank, everything could've been better. 


## Reflect on your personal experience. What are some of the ways that your team keeps services healthy and responsive? How do you test services? How do you react quickly to an unhealthy service?

I am a simple student that has never worked on any team before :(.  But in my future team, I'm sure i'll be sure to keep the concepts in mine

Add redundnacies so that no single point of failure will take down our service
Make sure our services are simplistic/operable/maintainable. To prevent bugs from appearing, and if they do appear, make it easy to fix.
As well as building our services with an expectation that that people operating then are no smarter then the gorilla that learned sign language. 
