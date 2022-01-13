# Chapter 8 Questions

## As a precursor to this chapter, read this twitter thread: [Cows causing outages](https://twitter.com/uhoelzle/status/1263333281891708929) and this article: [Sharks threaten Google's Fiber Optics](https://slate.com/technology/2014/08/shark-attacks-threaten-google-s-undersea-internet-cables-video.html) for some of the things that can go wrong outside of our control. In your experience, what are some other ways in which programming a networked application can go wrong?

The Internet is very fragile, and mainly connected by wires, but can
also be beamed from space (Starlink). Radio and telephones are a similar
system and face similar problems, as they can be beamed from space or on
land (where they can be taken down).

In space, there are many ways that a satellite can error out -- most
systems have 5+ computers on board that must sync with a majority of the
other computers before it makes a decision. These systems also try to be
remotely debuggable, since the suns rays can cause bit flipping or
invalidate SSD blocks.

On land, radio towers + above ground fiber towers are exposed to
political tensions (maybe the country shuts down the internet/radio,
which happens often), weather conditions (extreme snow, rain, fire,
wind, earthquakes), or dependencies (a utility company has an outage,
so the towers stop working), or human error (there are lots of human
operators that make sure telephone towers, radio towers, and internet
towers are operating correctly, but this can lead to errors).

In my experience as a dev, most of these things have happened except for
political instability. At my first job, where we self-hosted our
servers, our internet would go down often due to the utility company
going down or throttling us, the electricity would go out because of
heavy snow, rain, fires, and the servers would be down, and in other
jobs, AWS would go down due to some problems they had.

Lots of problems abound.

## What are the differences between cloud computing and supercomputing? What are some of the challenges inherent to cloud computing in particular?

Supercomputers are generally built with top of the line hardware,
whereas cloud computing tends to be backed by commodity hardware that is
connected through ethernet. As well, supercomputers are less likely to
be highly networked.

This makes the challenges that supercomputers have vs. cloud computing
different -- for a supercomputer, it's generally possible to assume
extreme reliability, since they aren't networked in the same way. For
example, early ATM systems had this assumption, where network partitions
would not happen due to the reliability of the parts and the ease of a
human operator to disable faulty nodes/synchronize the system.

In Cloud computing, the amount of downtime that would cause would be
unacceptable, and the parts are also more likely to be faulty. This
leads to architecting a system that's almost always broken in some way,
but providing low latency and requiring higher availability targets.

## If you make a request to a server and don't get a response, what are the possible failure modes? Why is this particularly challenging, coming from a single computer model?

1. DNS could be a problem, either on your client (you've cached the
   wrong record and need to update, but cannot) or on the server (the
   server has incorrectly setup its DNS, so it disappears from the
   internet).
2. The Internet is down. The client's internet could be down, or the
   server's.
3. There are some firewall settings that have changed. Maybe the client
   is firewalling the server, or vice-versa.
4. Either side may be having slowness, where the node cannot process the
   request.
5. Maybe the request did make it to the server, but the response didn't
   make it back to the client.
6. The node has failed. It is shut down or unplugged.
7. The response doesn't know where to go back to, as it travels through
   a load balancer or so.

## What is the difference between circuit based and packet switching networks? What are the pros and cons of each, and why isn't it possible for data intensive applications to use circuits?

Circuits are primarily used for telephone systems. Circuits allocate a
fixed buffer size for use, which allow for bounded delay times. Packet
Switching is used for the internet, where the packet can be of any size,
but suffers from exponential delay.

Since Data intensive applications are served over TCP/IP (most of the
time), we're bound to the packet switching paradigm. There was some work
to make applications run on circuits, but this can be inefficient and
hard to allocate for -- what happens if some 5% of your users use up 95%
of your bandwidth? You'd need to allocate as much bandwidth as the
biggest poweruser needs, which would be extremely wasteful, inefficient,
and probably not possible at scale. Thus, packet switching networks are
more efficient, but they come with the downside of having to think about
exponential backoff.

## What is the difference between time of day and monotonic clocks? (For a deeper dive on monotonic clocks, here's the erlang [docs](https://www.erlang.org/doc/apps/erts/time_correction.html). Are monotonic clocks a silver bullet? How might they fail in a networked application?

Time of day clocks return some number that returns the number of seconds
or milliseconds since the epoch (generally unix time). Time of day
clocks are synchronized with NTP over the network.

This can lead to some problems where Time of day problems jump forwards
or backwards in time (If the clock goes too fast, it will be
synchronized backwards, or if it goes too slowly, it will be
synchronized forward).

NTP being a networked time protocol, suffers the same problems as any
networked time protocol. As well, clocks in a computer can have large
variances in how accurately they can measure time, sometimes having a
lag of a couple ms per second. If servers don't sync with NTP often,
they can skew, leading to inaccurate times between nodes in a cluster.

Monotonic clocks are clocks that can only go forwards. A simple
implementation is a statically incrementing integer. The unit in which
the clock goes up is useless, but it shows that time is always going up.

```cpp
static atomic<size_t> time = 0;

void get_time() {
    return ++time;
}

size_t time_1 = get_time(); // 1
do_something();
size_t time_2 = get_time(); // 2
```

This sounds good, but in practice, can be rife with failures. Erlang's
implementation was buggy a few versions in the past, so monotonic clocks
went backwards when upgrading to a new version of Erlang, which could
break nodes in a cluster.

In general, monotonic time is a **better** abstraction for checking if
a set of actions was partially ordered, but it can still leak through to
the user.

## Read [NTP](https://en.wikipedia.org/wiki/Network_Time_Protocol) for a better idea of NTP. What are some of the ways NTP synchronized clocks may show the wrong time? Reflecting on your experience, have there been times where this has come to bite you?

NTP involves computers synchronizing with a set of other computers that
have the "correct" time, while adjusting for network latency and also
adjusting for the problems with the underlying hardware telling time.

NTP synchronized clocks might show the wrong time for a number of
reasons. On Apple's NTP servers, there have been times where clocks had
to be adjusted for leap seconds or go backwards to adjust for time based
discrepancies. This could easily break applications that expect
monotonic time from an NTP synchronized server.

I've had a few times where my computer synchronized to one NTP server
but my phone synchronized to another, so their times were slightly off
(< 1 minute). In general it hasn't been a big deal for me in distributed
systems, since I've never worked on an application that uses NTP time
for ordering (only ever a monotonic clock).

## [Spanner](https://cloud.google.com/spanner/docs/true-time-external-consistency) uses synchronized clocks in its data center to provide snapshot isolation. Feel free to read more about it on different sources on the web. What is novel about this approach compared to using a transaction id? What are some ways where it could create bottlenecks?

Spanner uses `TrueTime`, which is a set of very expensive clocks in a
datacenter for ordering. Truetime is used to assign timestamps to a
globally distributed database, so Spanner can perform consistent reads
across a globally distributed database without blocking writes.

This is novel because it allows you to treat spanner as if you're
treating a single node RDBMS, which allows for globally distributed
transactions without their abysmal performance but also with MVCC.

This could cause bottlenecks/correctness issues though if TrueTime goes
backwards in time, or if clients cannot query it properly. Instead of
using a transaction id and synchronizing that across all clients in a
distributed fashion, Spanner relies solely on its clock, so its
guarantees are only as strong as the precision of the clock.

## It may seem like GC pauses are insignificant, but there are cases where they can cause a large decrease in throughput, as experienced by [discord](https://discord.com/blog/why-discord-is-switching-from-go-to-rust). What are some ways to mitigate the impacts of GC pauses in applications? Have you used any of these in practice? If so, how did it go? If you'd like to read up on Java's GC algorithms, here's another resource: [Java's Garbage Collection Algorithms](https://ionutbalosin.com/2019/12/jvm-garbage-collectors-benchmarks-report-19-12/). What are the tradeoffs made by each GC algorithm? How could they impact the performance of a long running application?

1. GC pauses are insignificant for lots of applications.

There are many ways to reduce GC load, however, and most languages allow
you to play around with the GC algorithm they use.

2. Don't use a GC'ed language.

Discord moved from Go to Rust, achieving much better p999 latencies due
to the fact that there wasn't a GC to worry about anymore. Rust
generally uses RefCounting for Shared Resources. If you can get away
with very few Smart Pointers, allocating on the stack, and only using
unique pointers (Box in Rust), your application can get much faster.
However, RefCounting pauses are slow for shared data. In this case,
using a non-gced language won't help as much, since you have to resolve
cycles anyway.

3. Disable your GC.

Most languages (Ruby, Python, Java, C#) allow you to disable your GC
(this is useful for benchmarking consistently). As well, High Frequency
Traders might use a bump allocator which never frees memory, by
restarting the application every day to free memory. You can do this if
you can accept some non-zero amount of downtime, or are running some
kind of job where you never need to free memory (Compilers notoriously
use this technique where they only `malloc()` and never `free()`).

4. Use a different kind of GC

Most languages also offer different kinds of GC's -- Java has 10+
algorithms now, like bump, parallel ones, and stop the world ones. They
all have different tradeoffs and latencies, so just by using one that
matches your use case should improve your code's performance
significantly.

## What are some ways in which distributing a globally unique lock can fail? What are some ways to mitigate this in practice?

A distributed lock is required for many reasons, but mainly to prevent
split brain (two leaders think they are both the leader). It's generally
up to a service to achieve consensus. This can be done with a
monotonically increasing fencing token, where the lock service grants
any lease with a monotonically increasing number, and only accepts the
highest number it has seen recently.

## What is the byzantine generals problem? Why is it generally not important for applications that we work on? (Bonus: What is the cost of byzantine fault tolerance?)

The byzantine generals problem refers to the fact that if some nodes are
acting in a dishonest manner, trying to take down the system, this
fencing tokens approach doesn't work. It is generally costly to achieve
byzantine fault tolerance, and it can be assumed that nodes that you
operate in a system aren't trying to outright lie, so this is less
important for data intensive applications, but is important in NTP or
Blockchain technologies.

## How is a distributed algorithm determined to be correct in practice? What are some of the things to look out for?

Correctness is tough to verify -- Operational Transform algorithms in
the 80s and 90s that were purported to be correct turned out to have
edge cases. Likewise, proof solvers like TLA might return to you that an
algorithm is correct, but the implementors missed some assumption.
Basically, distributed systems can fail in new and unforseen ways, and
even simple analysis can not translate to the real world.

Another problem with a "correct" algorithm might be its performance,
which makes it unusable. For example, cuckoo hashing is correct and
terminates in constant time, but its overhead is larger than other
probing algorithms, so it isn't popular in practice. Likewise,
serializable reads are incredibly slow, so they are rarely used as well.
Wait-free algorithms, which terminate in a fixed number of steps are
also useful theoretically, but hard to use in practice (hardware support
isn't fast enough).
