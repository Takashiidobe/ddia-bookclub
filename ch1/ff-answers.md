# Chapter 1 Questions

## What does it mean for most applications to be data-intensive instead of compute-intensive?

Most applications are challenged by their data's sheer quantity, complexity, and rate of change. Contrast this with applications whose performance tracks with a CPU's power (think clock speed, caching features, temperature regulation, etc.).

## What is the definition of a reliable system? What are some of the ways that systems can become "unreliable"?

A reliable system should continue to perform its functions correctly at the desired level even under challenging conditions. 

Systems can become unreliable due to: 
- Human error e.g. incorrect operator configuration, prototyping "live in production", lack of unit or integration testing, inadequate deployment infrastructure, little to no monitoring capabilities, poor management and training practices
- Software failure e.g. application server crashes, runaway processes, corrupted or slow service responses
- Hardware failure e.g. drive crashes, faulty memory, power outage, disconnected cabling

## What makes a system scalable? What are the characteristics of a scalable system?

A scalable system should have reasonable methods to manage its growth in data volume, traffic, and complexity.

Characteristics of a scalable system include:
- Load Management
- SLAs
- Elasticity

## What is the difference between a horizontally scaling and vertically scaling system? What are some of the pros and cons for each?

Horizontal scaling involves distributing workload across multiple machines, whereas vertical scaling focuses on increasing capabilities of an individual machine utilized by the system.

||Horizontal scaling|Vertical Scaling|
||:---|:---|
|**Pros**|Cheaper individual machines|Simpler implementation|
||Cheaper to scale up on-demand||
|||
|**Cons**|More complex design|Expensive individual machines|
|||On-demand costs increase with addition of new machine|

## What makes a system maintainable? What makes a system "unmaintainable"?

A system is maintainable if:
- it is easy for ops to keep the system running smoothly (**operability**)
- new engineers can easily understand the system, due to a reduction in complexity where possible (**simplicity**)
- engineers can easily make changes to the system, adapting it to new use cases in response to requirements changes (**evolvability**)

A system is "unmaintainable" if:
- engineering cannot hand off the system to ops and trust the system to continue operating with any reasonable sense of consistency. 
- the current team cannot trust a future developer to understand the system's design as-is
- future engineers cannot extend or modify the system without potentially re-architecting the system

## Explain a project where you had to make it more reliable/scalable/maintainable. How did you do it, and what were some of the roadblocks in achieving the systems goals?

I am hardware man I no do this

Seriously though, closest example would be: updating a hardware design with rigid configuration at the circuit level. The FPGA collected data from a fiber optic network but the previous designer had not implemented any method for end users to change the sample size, the rate of data collection from the network, or what type of trigger would set off the collection process. 

My solution involved the addition of multiple "switches" and memory blocks that could store data from the Linux side of the board, allowing end users to pass commands to these switches on the FPGA even after the bitstream had already configured the logic gates. This made it easier for future users to use the FPGA without needing to understand how to write in Verilog, while also opening the door for future ASIC designs to come along and use the algorithms already in use on the board.

## Read three of the post-mortems in this [repo](https://github.com/danluu/post-mortems). What are some of the ways in which outages are caused? What could the teams have done to mitigate the blast radius or lowered the chance of such an outage happening? Explain the outage in your own words and give an explanation for what you learned from each summary.

Outages can have a variety of causes, but the common factor in all of them is usually a set of assumptions made by the end user(s) responsible for the system's automated decision-making. These baseline assumptions that come even before implementation can affect how these systems are designed, leading to catastrophic failures that could have been avoided. These outages also tend to be caused by a single traceable point of failure that stems from a variety of issues - like behavior validation for one piece of software occurring within that piece of the software itself. Validation outside of the feature itself could help developers know that their work has another set of eyes by technical and organizational design.

#### https://status.cloud.google.com/incident/compute/15056#5719570367119360
This outage was caused due to the heavy load on a subset of machines focused on storing the most recent writes to GCP. This led to instances where said data was committed to storage that required manual retrieval, or was corrupted and lost.

Some ways that GCP could have mitigated this:
- Rotate and intermix machines that handle recent data writes to avoid hardware fatigue
- Increase the number of machines that handle recent data writes
- Develop automated methods for checking data integrity on said machines in the event of a system failure
- Automate retrieval process for data in stable storage

#### https://azure.microsoft.com/en-us/blog/summary-of-windows-azure-service-disruption-on-feb-29th-2012/
This disruption was caused due to a bug in the transfer certificate generator of the Guest Agent (GA) in Azure, in which leap days (February 29th) were assumed to be valid in any given year. When a certificate was generated on Feb. 29th, 2012, the date that the certificate was deemed valid through was Feb. 29th, 2013 - which does not exist. Any time such a certificate was generated, it would fail due to the invalid date, causing the GA to terminate. This lead to a series of resets that cause the system to assume a hardware failure for a given cluster, requiring human intervention.

Some ways Azure could have mitigated this:
- Validate the content of certificates (separately from the transfer certificate generation process) on the GA side
- When failures are detected, document the failure for human operators to view
- With software features like this that can be unit tested, thoroughly unit test for edge cases and common errors beyond MCDC coverage.

#### https://users.csc.calpoly.edu/~jdalbey/SWE/Papers/att_collapse.html
This failure was caused by a bug in the message handling software between switches on AT&T's network. When two messages came in back-to-back, the software would begin handling the first message but inadvertently fail with the second message due to the logic in the case statement the message handler used, breaking out of the logic that would have processed this second message. This led to the message being overwritten by the subsequent operations performed by the program, leading to cascading failures throughout the system since the logic applied to all of the switches in the network at the time.

AT&T could have avoided this by:
- Testing these feature changes towards the end of their hardware and software's limits, to detect edge cases
- Rolling out changes slowly through the network, potentially in pairs of switches at a time
- Isolate and log automated mechanisms (such as routine maintenance procedures) to improve visibility for operators to track over time

## Reflect on your personal experience. What are some of the ways that your team keeps services healthy and responsive? How do you test services? How do you react quickly to an unhealthy service?

N/A. My team is not service-oriented, and all of our processes are slower than molasses - whether it comes to design, development, deployment, or fixes.
