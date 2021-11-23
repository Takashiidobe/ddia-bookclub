# Chapter 8 Questions

## As a precursor to this chapter, read this twitter thread: [Cows causing outages](https://twitter.com/uhoelzle/status/1263333281891708929) and this article: [Sharks threaten Google's Fiber Optics](https://slate.com/technology/2014/08/shark-attacks-threaten-google-s-undersea-internet-cables-video.html) for some of the things that can go wrong outside of our control. In your experience, what are some other ways in which programming a networked application can go wrong?

-- YOUR ANSWER HERE --

## What are the differences between cloud computing and supercomputing? What are some of the challenges inherent to cloud computing in particular?

-- YOUR ANSWER HERE --

## If you make a request to a server and don't get a response, what are the possible failure modes? Why is this particularly challenging, coming from a single computer model?

-- YOUR ANSWER HERE --

## What is the difference between circuit based and packet switching networks? What are the pros and cons of each, and why isn't it possible for data intensive applications to use circuits?

-- YOUR ANSWER HERE --

## What is the difference between time of day and monotonic clocks? (For a deeper dive on monotonic clocks, here's the erlang [docs](https://www.erlang.org/doc/apps/erts/time_correction.html). Are monotonic clocks a silver bullet? How might they fail in a networked application?

-- YOUR ANSWER HERE --

## Read [NTP](https://en.wikipedia.org/wiki/Network_Time_Protocol) for a better idea of NTP. What are some of the ways NTP synchronized clocks may show the wrong time? Reflecting on your experience, have there been times where this has come to bite you?

-- YOUR ANSWER HERE --

## [Spanner](https://cloud.google.com/spanner/docs/true-time-external-consistency) uses synchronized clocks in its data center to provide snapshot isolation. Feel free to read more about it on different sources on the web. What is novel about this approach compared to using a transaction id? What are some ways where it could create bottlenecks?

-- YOUR ANSWER HERE --

## It may seem like GC pauses are insignificant, but there are cases where they can cause a large decrease in throughput, as experienced by [discord](https://discord.com/blog/why-discord-is-switching-from-go-to-rust). What are some ways to mitigate the impacts of GC pauses in applications? Have you used any of these in practice? If so, how did it go? If you'd like to read up on Java's GC algorithms, here's another resource: [Java's Garbage Collection Algorithms](https://ionutbalosin.com/2019/12/jvm-garbage-collectors-benchmarks-report-19-12/). What are the tradeoffs made by each GC algorithm? How could they impact the performance of a long running application?

-- YOUR ANSWER HERE --

## What are some ways in which distributing a globally unique lock can fail? What are some ways to mitigate this in practice?

-- YOUR ANSWER HERE --

## What is the byzantine generals problem? Why is it generally not important for applications that we work on? (Bonus: What is the cost of byzantine fault tolerance?)

-- YOUR ANSWER HERE --

## How is a distributed algorithm determined to be correct in practice? What are some of the things to look out for?

-- YOUR ANSWER HERE --

