# Chapter 6 Questions

## What is the purpose of partitioning? How does it compare to indexing?

Partitions divide records to nodes in the system. Instead of keeping all
records on one node, and replicating like in the last chapter, some
nodes contain all of the records in the system.

This allows for increased write volumes at the expense of reads (joins
in particular become extremely problematic).

Indexing can be thought of as the inverse: Indexes speed up reads at the
cost of writes, whereas partitioning speeds up writes at the cost of
reads.

## Two strategies to partition data include by key range and by hash. What are the pros and cons of both approaches? Could you imagine any cases where either or both would perform well? Poorly?

1. By Key Range

By Key range ends up poorly scaling: certain shards can become hot spots
for your data, and might experience more reads and writes than other
nodes, which can lead to failover times.

2. By Hash

Assuming we have a close to perfect hash function (any input is randomly
distributed in the set), hashing might seem to work well, but
unfortunately, since hashes are not ordered, range queries no longer
work if we use a hash function.

## What are the strategies for partitioning via secondary indexes? How do they differ in their implementation? What are their pros and cons?

In an RDBMS, secondary indexes are on the same node, so they work just
like normal indexes would.

If nodes are partitioned, then there are two possible secondary indexes:
global and local.

A global index involves one index where all nodes can write to.

Imagine we have 2 nodes, and there is an index on red cars.

A global secondary index would receive a write every time either of the
two nodes receives a write for a red car, but this happens
asynchronously (which means stale reads may occur).

A local index would mean that all nodes keep an index for all records
they own, and to do a query for all red cars, the application must read
every node (otherwise it may miss a record on another node).

## What are some strategies for rebalancing partitions? Why are some discouraged? What is the difference between automatic and manual rebalancing?

1. Hash mod N

Don't do this. If the number of nodes increases because the system has
increased load, all records must be rehashed and moved to another node.
You can imagine a vector: if it overflows its size, it must copy all of
its items to a vector that is twice its size, which costs O(n) time.

2. Fixed number of partitions

Create way more partitions than there possibly can be, and then have the
nodes take their nearest partitions until all partitions are taken care
of.

When a new node is added, this new node can take a partition(s) from
node(s) until the system is fairly balanced again.

The opposite works for a node leaving the system: the node leaving the
system can distribute its partitions to nodes and then leave the quorom.

3. Dynamic Partitioning

Let the database do the work! It'll figure out the partitions. This
works pretty well, but still requires intervention, because the database
doesn't have a lot of context on the data, and which part may become
hotspots.

4. Partitioning proportionally to nodes

Cassandra does this: the system creates a number of partitions that is
proportional to the number of nodes. If you increase the number of
nodes, the partitions become smaller, and vice versa. When a new node
enters the cluster, it splits heavy nodes in half and takes ownership of
those nodes.

This looks like it falls into the same trap as Hash mod N, but in
reality, it does not, because there are a large amount of partitions, so
unfair splits are very rare in practice.

Automatic rebalancing involves letting the software choose the number of
partitions and rerouting requests thusly. This can lower the amount of
operational overhead, but can result in hotspots.

## Reflect upon your own personal experience. Have you worked on a system that has automatic or manual rebalancing (or both?). Why did you end up choosing automatic or manual rebalancing, or a hybrid approach?

I've only worked on systems with manual rebalancing for two reasons:

1. Rebalancing is expensive/complicated.
2. Rebalancing is a rare event.

There's a lot of tooling built around making sure that rebalancing
correctly works (given the amount of state that interacts with a given
partition) but it must be fired off by an operator.

It is important to have lots of monitoring on rebalancing as well, to
make it possible to undo and check for problematic updates.

In our case, automatic rebalancing would've been risky, but monitoring
is in place to let us know about CPU usage and disk usage on certain
partitions well in advance of certain limits being reached, so SREs can
make the decision to move to another partition if required.

## What is service discovery in the context of replication? Choose one implementation of request routing (e.g. Zookeeper, etcd, GFS) and explain how it works under the hood. (ZAB for ZK, raft for etcd, or Paxos for GFS). What are some of the tradeoffs made by the algorithm you chose? In your own words, name the strengths of this algorithm compared to other algorithms.

I chose ZAB (Zookeeper's algorithm) because I've used it before.

Zookeeper handles leader election, service discovery, and
synchronization of transactions.

ZAB does this by using a two-phase commit protocol, with casual
ordering, dependable delivery, and total ordering guarantees.

Basically, ZAB favors consistency over availability, as if it cannot
find a quorom of nodes to persist a transaction, it will decline to
acknowledge that the transaction succeeded.

Zookeeper provides sequential consistency, which is close to
linearizability, but offers a `sync` command, which allows for a
strongly consistent read, as it syncs the nodes together for a read, but
this will fail unless a majority of nodes can be reached.

Thusly, this is a less performant consensus algorithm that favors consistency.

## This chapter has a fairly light explanation of replication for now, which will be covered more in detail later on in the third part of the book. What are some of the questions you had while reading the chapter? If you have any, try to search around and see how they could be addressed. If a cursory look didn't uncover a satisfactory answer, feel free to ask about your question in the discord.

1. How are writes scaled in a distributed system?

In my eyes, writes can be scaled by adding more nodes and partitioning
for an RDBMS, but this comes at the cost of range query reads becoming
slower (reads over a range may require querying multiple partitions,
which doesn't scale anymore).


