# Chapter 1 Questions

## What does it mean for most applications to be data-intensive instead of compute-intensive?

The application is no longer restricted to the CPU. It is restricted by the amount of data, the complexity of data, and the speed the data is changing.

## What is the definition of a reliable system? What are some of the ways that systems can become "unreliable"?

A reliable system is a system that will work correctly even if there are hardware or software faults, and human errors.
The way systems can become unreliable are if we dont build fault tolerant or resilient systems. 

## What makes a system scalable? What are the characteristics of a scalable system?

A scalable system are the ability to cope with increase load. The characteristics of a scalable system are built from general purpose building blocks, arranged in patterns that are familiar.

## What is the difference between a horizontally scaling and vertically scaling system? What are some of the pros and cons for each?

The difference in between horizontally scaling and vertically scaling is horizontal scaling is getting multiple machines to distribute the load while vertical scaling is getting a singular powerful computer. The pros of a vertical scaling is they are usually a simpler. the cons of such scaling in its very expensive, and very intenstive loads can't avoid scaling out. One con for horizontal scaling is they induce a ton of complexity. the pros of horizontal scaling is they are good at handling large volumes of data or traffic. 
There is no one size fit all solution. very dependent on application.

## What makes a system maintainable? What makes a system "unmaintainable"?

There are three things that make a system maintainable. 
Operability - Make it easy for operation teams to keep the system running smoothly
Simplicity - Reduce complexity as much as possible from the system.
Evolvability - Make it easy to make changes in the future. Adapt for unanticipated use cases. Also, known as extensibility, modifiability, plasticity.

Things that make a system unmaintainable are, lots of complexitiy, terrible visibility into the system's health, not able to adapt to new cases.

## Explain a project where you had to make it more reliable/scalable/maintainable. How did you do it, and what were some of the roadblocks in achieving the systems goals?

Aint never had to cuz. Becuz i am a nooby nerd that sucks.

## Read three of the post-mortems in this [repo](https://github.com/danluu/post-mortems). What are some of the ways in which outages are caused? What could the teams have done to mitigate the blast radius or lowered the chance of such an outage happening? Explain the outage in your own words and give an explanation for what you learned from each summary.

https://aws.amazon.com/message/2329B7/
The outage was occured because of a failure in a transformer. Which resulted in loss of electricty. Once the loss of electricy happened the generators kicked on and one of the PLCs to sync the the generators failed. The reason the PLC failed is because of a ground fault detected made it fail its task. The way to mitigate this is add redundacy to the PLCs and isolation of such equipment in a stable enviroment. have redudancy and check on shit.

https://status.discordapp.com/incidents/dj3l6lw926kl

An internal service at discord which tracks real time state and information of users disconnected from its cluster. Another node which handles a different service tried to reconnect causing a "thundering herd" towards a cluster. which caused 1/3rd of all connected sessions on Discord to effectively stall, queuing events in memory. then ran out of memory and crashed. the way to fix was fixing the split brain bug. also, limited the connections to sessions cluster to presence server. wish people deployed bug patches sooner that you told people about

https://status.heroku.com/incidents/642?postmortem

The issue started a rolling restart of our runtime agents. This caused a loss of communication between the runtime agents on the updated instances and the dyno manager. While dynos kept running, the dyno manager stopped receiving status updates and was unable to start new dynos, restart existing dynos or respond to dyno crashes and other types of failures. This caused some apps to experience routing issues when dynos crashed. Fix the operational documentation. also, maintenance scheduling. Write good docs 

## Reflect on your personal experience. What are some of the ways that your team keeps services healthy and responsive? How do you test services? How do you react quickly to an unhealthy service?

personal experience? ah good one chief.
But one way to be able to respond to an unhealthy system is having a quality monitoring software to monitor the system and act upon the issue. Also, having good documentation on what to do in which circumstance.
