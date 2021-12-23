# Chapter 5 Questions

## Explain some of the reasons developers replicate data. What are some things to keep in mind when replicating data? How does it change how applications read and write data?

Replication means keeping more than one copy of the data. Applications
may do this to:

1. Keep a copy of the data closer to users (the speed of light is only
   so fast).
2. To increase availability (if one machine is down, maybe another can
   serve it?)
3. To scale out the number of reads the system can issue.

Some things to keep in mind while replicating data are the following:

If writes are asynchronously replicated, the application must understand
that reads can be stale. If nodes crash, previously acknowledged writes
can also disappear, which is difficult to deal with on the application
side.

If write are synchronously replicated (semi or otherwise), then writes
will take much longer than they should (2+ writes + 2 + network
connections). Semi-synchronous replicated applications suffer from high
latency at tail values (p99+) and in some cases writes will fail, which
the application will need to keep in mind.

## Explain leader-based replication in your own words. What are some of the strategies to replicate data between instances? What are their pros and cons?

Leader based replication involves nominating one node as a "leader",
which is the only node that will accept writes. All writes are sent to
this leader, and the leader is responsible for replicating the content
of these writes to its followers in some way.

There are three general ways to replicate data in this fashion:

1. Asynchronously (Best performance, can lose writes, has stale reads,
   can tolerate any node being down except the leader)
2. Semi-synchronously (Medium performance, does not loses writes, can
   have stale reads, cannot accept writes if synchronously replicated
   node is down)
3. Synchronously (worst performance, does not lose writes, no stale
   reads, cannot accept writes if one node is down)

Because of these conditions, synchronous replication is rarely used in
practice, even though it is the simplest for application programmers to
understand (It sacrifices availability for consistency).

However, even though asynchronous replication has great performance, it
can lose writes if the leader fails without its writes being propagated.
This is a poor experience for the application programmer as well, as
writes they thought had propagated had not, in reality, been durable.

## Assume the leader falls over. What are some of the ways in which a new leader can be chosen? What are some of the troubles in selecting a new leader?

If the leader fails, a new leader must be elected. This is called leader
election, and generally involves the remaining nodes voting the most
caught up node to leadership. This can have some troubles:

1. We first need to determine that the leader has failed. If the leader
   is accepting writes but has been partitioned from the rest of its
   followers, it may still accept writes, thinking it is the leader,
   while the other nodes elect another leader that also takes writes.
   This is called "Split Brain" and will always lead to lost writes.
2. Leader election has to do something with lost writes. Should it throw
   them away? Should it commit them? What does it do with state the
   leader might have but the followers may not?
3. The leader must now start replicating writes to its new followers.

## What are the four main ways to replicate databases? What are the pros and cons of each? If you work on a system that requires replication, which one do you use and why?

1. Statement based replication
   Statement based replication involves sending over the SQL statements
   that make up each query from leaders to followers.
   This is extremely perilous, but extremely efficient because shipping SQL
   statements are much cheaper than something like row-based
   replication.
   MySQL allows for both statement-based and row-based replication, by
   shipping statements if they are deterministic, and otherwise shipping
   the rows if not. This functionality is buggy, though, because at
   compile time it is not possible to determine whether or not an SQL
   statement is deterministic, so this method is discouraged.
2. WAL shipping
   WAL shipping involves the leader sending its followers bytes from its
   WAL along with transaction IDs.
   This is decently efficient (WALs are a binary format) and
   deterministic (followers can not get in an invalid state). Postgres
   and MySQL can do this: but be careful, if transaction ids overflow
   (i.e. go over their allotted 32 bit number of ~4B, transactions will
   stop replicating). As well, the WAL can change between major version
   bumps, so downtime may be required when updating your database
   version.
3. Row Based replication
   Row based replication involves sending over a primary key for a row
   and all of its changed contents. This is the most common way of
   replicating, as it is deterministic, but can be slower, since it
   involves shipping a whole row to the rest of the system.
4. Trigger-based replication
   Trigger based replication is done by having triggers replicate each
   write. This is extremely slow and is prone to bugs. That being said,
   it can be useful because only a set of changes can be replicated and
   triggers can be quickly applied and removed.
5. Application based replication
   The application replicates writes from one place to another. For
   example, if you'd like to start migrating data from one database to
   another, the above ways of replicating don't work because the
   databases may not speak the same protocol over the wire (let's say
   you're trying to move from Postgres to MongoDB). The application can
   handle writing to both stores, as a way of replication. This is very
   commonly done with feature flags and only for a subset of writes. As
   well, since the application layer is stateless, this can be done at
   any time, and feature flags can be disabled or enabled by operators,
   making this easy to do for most applications.

## Read this page in its entirety [Consistency Models](https://jepsen.io/consistency). Is there anything that you didn't expect? Choose one of the consistency models on this page and explain in your own words what it does, and note some things to be careful of when using it.

I chose "Read Committed", an ANSI SQL consistency model.

Read Committed can be totally available, with a performance cost.
However, this doesn't enforce a strict ordering for reading your writes:
A transaction that is committed might not be readable even after it is
committed.

## Have you ever been the victim of replication failures? If so, explain what happened, and what you suspected happened on the back-end. The author once went shopping for groceries at his local grocery store, and noticed that even though he clicked to add a bag of white rice, it was not added to his cart or receipt. When the author received said groceries, he had 8 bags of rice, one for each time he added a bag of rice to his cart.

I was the author. I had to eat that rice. I gained 5 pounds.

## Compare and contrast multi-leader replication and single leader replication. Take an application that you use every day that is multi-leader (e.g. git, google docs, a cloud synced notes app) and explain how you would implement it in your own terms, using the concepts touched upon in this chapter. What are the performance and reliability characteristics of your approach? Why did you choose one in particular over others?

I'll pick google docs.

Google docs uses operational transformations to allow for multiple
writers on the same document.

A text editor involves using a data structure called a rope (A tree like
data structure, that allows for log n operations anywhere) where every
line is a node in the tree, and the nodes are roughly sorted.

For writes, each author may choose a location to write to (which
appends) to it or deletes from the line. If there are any conflicts,
like deleting the same lines, overlapping deletes are thrown out, but
if writing, both appends might succeed. For updates, there may be some
conflicts, but you can choose to allow one person's writes to succeed
based on RNG.

This is roughly how google docs works in practice, but it comes with
restrictions: it does not scale infinitely, since having more than 1000
users on a document disables writing to it (because 1000 writers at the
same time will crash the server) and some conflicts might not make sense
(if two people think the same thing and roughly type the same thing, it
is typed twice even though it should've only been typed once).

If I chose to write google docs myself, I'd choose to use CRDTs
<https://www.inkandswitch.com/peritext/> to do the same. This would
allow me to offload the complexity to a library while being performant.

## Read the section on replication topologies. What are the pros and cons of each? Can you find an example of each topology in real life? What are some of the constraints that the topology imposes on the system? (Think about how fortnite keeps the same state between 100+ players in 16ms, or how call of duty uses one player's computer as a central server, or how torrenting networks keep file integrity, so malware does not spread across the network).

There are three main topologies: circular, star, and all-to-all.

Circular topology involves each node being a leader and follower to the
next node in the system.

This fails when one node dies, so this doesn't work out in practice.

A star topology involves a single leader, with all nodes talking only to
the leader.

An all-to-all topology involves multiple leaders, which can all accept
writes.

For something like fortnite, all writes need to be replicated throughout
the system without being centralized, something like an all-to-all
topology might work.

## What is leaderless replication? What is an example of a database that uses leaderless replication? What are some restrictions on choosing quorum size, and what are the problems that arise?

Leaderless replication involves some quorom of the nodes acknowledging a
write. Reads involves reading from some quorom of the nodes
acknowledging a read.

This makes reads and writes slower than they would be from a system with
leader replication, but is about as fast for writes.

This means that writes can fail if a quorom isn't reachable (like in
semi-synchronous writes to a system with leaders) and reads can be stale
(if the read is not issued to the correct node).

Dynamo is a database that uses this setup, with a gossip protocol to
propagate reads and writes. This setup prioritizes availability, but a
large amount of reads can be stale.

To calculate how to quorom works, we can use three variables:

- `n`, the number of nodes
- `w`, the number of nodes that need to acknowledge a write
- `r`, the number of nodes that need to acknowledge a read

We can setup a quorom where the following holds:

- `w < n` allows for writes if a node is unavailable.
- `r < n` allows for reads if a node is unavailable.

If `w + r > n`, every read is consistent, but might not be available.

By setting `w` and `r` to lower numbers, there may be stale reads but
there's higher performance.

## Read the section on concurrency, time, and replication. How do distributed systems determine causal dependencies (a happened before b). Is this similar to a graph algorithm you know? If you read the paper [Time, Clock, and the Ordering of Events in a Distributed System](https://lamport.azurewebsites.net/pubs/time-clocks.pdf), what is required to create a partial ordering (a happens before b) of events in a distributed system? What are the tradeoffs of the method proposed by this paper?

To create a partial ordering of happened before, the paper uses lamport
timestamps. Lamport timestamps invoke total ordering for events that
could occur concurrently by using a numbered timestamp for sending and
receiving events.

Lamport timestamps can only return an order if there's a causal
relationship between two events: if they haven't exchanged messages,
then the events can be considered concurrent.

Unfortunately, with lamport timestamps, we can't determine if there is a
causal relationship between two events.
