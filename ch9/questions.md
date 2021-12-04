# Chapter 9 Questions

## Explain Eventual Consistency in your own words. What factors make it hard to reason about?

-- YOUR ANSWER HERE --

## Explain Linearizability in your own words. What makes it so desirable to application developers?

-- YOUR ANSWER HERE --

## What is the difference between linearizability and serializability?

-- YOUR ANSWER HERE --

## Assume we are running a hotel called CSCH (Computer Science Community Hotel) where users can book a room to stay for the night. If the system is linearizable, what would happen in case of a race condition where two users booked the same room at the same time? How should this be handled? In the case of a non-linearizable system, where two people can book the same room for the same night, what are some ways that the hotel can cope with two people booking the same room for the same night?

-- YOUR ANSWER HERE --

## Explain how a system without linearizability might run into problems. How would linearizability solve those problems?

-- YOUR ANSWER HERE --

## In your own words, explain which types of replication are linearizable. For each one, explain why or why each type of replication can be made linearizable or cannot be made linearizable.

-- YOUR ANSWER HERE --

## Explain the CAP theorem in your own words. Come up with an example of a system where you can choose to design for either linearizability or availability.

-- YOUR ANSWER HERE --

## What is the performance cost of linearizability? Can you come up with a system that traded off linearizability for performance reasons?

-- YOUR ANSWER HERE --

## What is the difference between Total Ordering and Partial Ordering? For an implementation in a programming language, read Rust's documentation on [Total Ordering](https://doc.rust-lang.org/std/cmp/trait.Ord.html) and [Partial Ordering](https://doc.rust-lang.org/std/cmp/trait.PartialOrd.html). Give an example of a system you know that has total ordering, and one that only provides partial ordering. What are some of the problems that come with each?

-- YOUR ANSWER HERE --

## Explain Lamport timestamps in your own words. What is an example of an operation which can fail when using them?

-- YOUR ANSWER HERE --

## What are the guarantees of Total Order Broadcast? Explain an operation in your own words that becomes correct with total order broadcast.

-- YOUR ANSWER HERE --

## Explain Two Phase Commit in your own words. How does it provide a guarantee for atomic transactions? What happens if the coordinator crashes?

-- YOUR ANSWER HERE --

## What are the requirements for Fault-Tolerant Consensus? What are some of the characteristics of a fault tolerant consensus algorithm?

-- YOUR ANSWER HERE --

## Dive deep on a particular consensus algorithm that you're interested in. Explain how the algorithm meets the requirements for fault tolerance and discuss the tradeoffs of its design decisions. Finally, describe what sort of performance tradeoffs the algorithm is ideal for.

-- YOUR ANSWER HERE --

