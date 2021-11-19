# Chapter 1 Questions

## What does it mean for most applications to be data-intensive instead of compute-intensive?

Applications with an execution time that is dominated by I/O or
manipulation of data are considered data-intensive. Applications that
spend most of their time on the execution of machine instructions are
compute intensive.

Most web applications are data-intensive, and systems software + real
time software tends to be compute-intensive.

## What is the definition of a reliable system? What are some of the ways that systems can become "unreliable"?

A reliable system works correctly in the face of adversity. A system
should perform the way a user expects (the principle of least
surprise), correct/guide the user when they make mistakes, and prevent
invalid states.

As well, a system that tolerates faults tends to be more reliable, since
systems tend to be in a state where something is going wrong, namely
hardware faults (bits can be flipped, data can be lost on a machine, a
machine cannot be reached through the network). A system that can
tolerate the loss of one machine is much more reliable than a system
that goes down when a machine goes down.

## What makes a system scalable? What are the characteristics of a scalable system?

A scalable system is one that can handle increased load without
sacrificing correctness and reliability. Some of the characteristics of
a scalable system are:

- Doing as little work as possible
- Relying on fewer dependencies
- Caching expensive work when possible

## What is the difference between a horizontally scaling and vertically scaling system? What are some of the pros and cons for each?

- vertical scaling = Buying a more powerful machine
- horizontal scaling = Distributing load across multiple smaller
machines

Vertical scaling has the pro that all of the state is kept on one
machine: Networked games, for example (e.g. Halo) would buy one
expensive server and put all of the server state on that machine, which
users would be able to query. This is efficient since players only have
to go to one server, but there is a hard cap on scale
(you may be able to put a million players on one server, but can you put
a billion?). As well, reliability can be hard to guarantee since there
is normally only one machine, so when that falls over, the system is no
longer available.

Horizontal scaling has the pro that it is easier to spin up a new node
in the graph, and the failure of one node doesn't take down the whole
system. That being said, having a graph of dependencies increases
complexity, and increases the total amount of work the system needs to
do -- there needs to be some way to store redundant information. As
well, state requires careful thought, since nodes are more likely to go
up and down, losing state in the process -- the system needs to be
made in a way that state is persisted and there's no way to get into an
invalid state.


## What makes a system maintainable? What makes a system "unmaintainable"?

A maintainable system is one that is easy to run, simple to understand,
and easy to extend. Any system that does not fit those criteria are less
maintainable.

Some characteristics of a maintainable system involve:

- Monitoring data/restoring services that go into a bad state
- Maintaining security of the platform
- Defining processes around what to do when things go wrong (runbooks)
- Anticipating future problems
- Keeping the system as simple as possible.

Unmaintainable systems do the opposite.

## Explain a project where you had to make it more reliable/scalable/maintainable. How did you do it, and what were some of the roadblocks in achieving the systems goals?

- One project to make a system more maintainable involved swapping out
the usage of MySQL locks for Zookeeper locks. MySQL locks were likely to
reject writes on a particular row if there was already a write that had
occurred in that row. Therefore, moving to Zookeeper locks would reduce
contention, since they spinlocked for less time, and were easier for the
application program to query for. We made the change and we found our
system avoided invalid states (where the user thought their write was
persisted, but the database had rejected the write).

## Read three of the post-mortems in this [repo](https://github.com/danluu/post-mortems). What are some of the ways in which outages are caused? What could the teams have done to mitigate the blast radius or lowered the chance of such an outage happening? Explain the outage in your own words and give an explanation for what you learned from each summary.

1. When a leap second occurred, `CLOCK_REALTIME` was rewound by one
   second. This led to applications that slept for less than one second
   to spinwait without sleeping, causing increased CPU load.

This being an OS related bug, is harder to catch. There could've been
fuzzing done to catch what would've happened on applications that slept
for less than a second, but in general, time related bugs are hard to
catch. Don't trust your OS -- Erlang has had bugs where the VM broke
monotonic time, taking down applications.

2. Mandrill: Transaction ID wraparound in Postgres caused a partial outage lasting a day and a half.

A transaction ID wraparound occurred in Postgres, because transaction
IDs in postgres use IDs from 0 to 2^32. Postgres doesn't aggressively
vacuum dead tuples, and asynchronously doing this in the background can
fall behind the creation of new tuples if the database is under high
load. Shard4 of Mandrill fell behind, and the database needed to be shut
down in order to vacuum dead tuples. This ended up taking 1 and a half
days.

Mitigation would've involved spinning up more nodes (shards) and
monitoring each shard to mitigate against transaction id exhaustion. As
well, Postgres' dead tuple vacuuming is notoriously slow -- hence why
postgres nodes should have a lot of spare CPU usage, moreso than MySQL
or other databases.

3. A configuration change that was supposed to address increased usage
   of a metadata service ended up overloading the BLOB lookup system,
   which caused a cascading failure on other services, like GMail,
   Photos, Drive, and GCP services that relied on this metadata
   service.

Metadata services tend to cause lots of grief since they are relied upon
by many other nodes in the system. As well, configuration changes are
very tricky since they can't be tested in any way, outside of staging,
where circumstances may not be the same as in production.

A routine configuration change can knock down services in ways that you
might not expect, and when it comes back up, because of queueing, the
same service can die again because it is overwhelmed by traffic that
other services send to it. Systems have to find a way to drop either
unimportant traffic or all traffic in order to prevent increased load
and other parts of the system from falling down.

## Reflect on your personal experience. What are some of the ways that your team keeps services healthy and responsive? How do you test services? How do you react quickly to an unhealthy service?

Some of the ways teams I've worked on in the past have made sure our
systems are correct involve fuzzing, monitoring dependencies
(dependabot, snyk), pen testing, chaos testing, smoke tests, and the
like.

In order to react quickly to unhealthy services, we have lots of
monitoring and PagerDuty set up so that if a service goes down or acts
in a degraded or inconsistent matter, somebody can work to fix the
problem. As well, the ability to quickly rollback changes has saved us
many times, with incremental rollouts.
