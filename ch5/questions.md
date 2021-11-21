# Chapter 5 Questions

## Explain some of the reasons developers replicate data. What are some things to keep in mind when replicating data? How does it change how applications read and write data?

-- YOUR ANSWER HERE --

## Explain leader-based replication in your own words. What are some of the strategies to replicate data between instances? What are their pros and cons?

-- YOUR ANSWER HERE --

## Assume the leader falls over. What are some of the ways in which a new leader can be chosen? What are some of the troubles in selecting a new leader?

-- YOUR ANSWER HERE --

## What are the four main ways to replicate databases? What are the pros and cons of each? If you work on a system that requires replication, which one do you use and why?

-- YOUR ANSWER HERE --

## Read this page in its entirety [Consistency Models](https://jepsen.io/consistency). Is there anything that you didn't expect? Choose one of the consistency models on this page and explain in your own words what it does, and note some things to be careful of when using it.

-- YOUR ANSWER HERE --

## Have you ever been the victim of replication failures? If so, explain what happened, and what you suspected happened on the back-end. The author once went shopping for groceries at his local grocery store, and noticed that even though he clicked to add a bag of white rice, it was not added to his cart or receipt. When the author received said groceries, he had 8 bags of rice, one for each time he added a bag of rice to his cart.

-- YOUR ANSWER HERE --

## Compare and contrast multi-leader replication and single leader replication. Take an application that you use every day that is multi-leader (e.g. git, google docs, a cloud synced notes app) and explain how you would implement it in your own terms, using the concepts touched upon in this chapter. What are the performance and reliability characteristics of your approach? Why did you choose one in particular over others?

-- YOUR ANSWER HERE --

## Read the section on replication topologies. What are the pros and cons of each? Can you find an example of each topology in real life? What are some of the constraints that the topology imposes on the system? (Think about how fortnite keeps the same state between 100+ players in 16ms, or how call of duty uses one player's computer as a central server, or how torrenting networks keep file integrity, so malware does not spread across the network).

-- YOUR ANSWER HERE --

## What is leaderless replication? What is an example of a database that uses leaderless replication? What are some restrictions on choosing quorum size, and what are the problems that arise?

-- YOUR ANSWER HERE --

## Read the section on concurrency, time, and replication. How do distributed systems determine causal dependencies (a happened before b). Is this similar to a graph algorithm you know? If you read the paper [Time, Clock, and the Ordering of Events in a Distributed System](https://lamport.azurewebsites.net/pubs/time-clocks.pdf), what is required to create a partial ordering (a happens before b) of events in a distributed system? What are the tradeoffs of the method proposed by this paper?

-- YOUR ANSWER HERE --

