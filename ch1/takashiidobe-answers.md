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


## What is the difference between a horizontally scaling and vertically scaling system? What are some of the pros and cons for each?

-- YOUR ANSWER HERE --

## What makes a system maintainable? What makes a system "unmaintainable"?

-- YOUR ANSWER HERE --

## Explain a project where you had to make it more reliable/scalable/maintainable. How did you do it, and what were some of the roadblocks in achieving the systems goals?

-- YOUR ANSWER HERE --

## Read three of the post-mortems in this [repo](https://github.com/danluu/post-mortems). What are some of the ways in which outages are caused? What could the teams have done to mitigate the blast radius or lowered the chance of such an outage happening? Explain the outage in your own words and give an explanation for what you learned from each summary.

-- YOUR ANSWER HERE --

## Reflect on your personal experience. What are some of the ways that your team keeps services healthy and responsive? How do you test services? How do you react quickly to an unhealthy service?

-- YOUR ANSWER HERE --
