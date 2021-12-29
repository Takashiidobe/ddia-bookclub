# Chapter 10 Questions

## What are the three types processing systems that Kleppmann mentions? Explain their differences in your own words and give an example of a system/app for each one.

-- YOUR ANSWER HERE --

## What is the Unix Philosophy? Google/come up with an example of a batch processing job that you might use in your work, and explain how it works and why it may/may not be fit for certain problems. What are some of the perks of the Unix Philosophy? What are some cons?

(For a fun story about the Unix philosophy, Mcllroy (who suggested to
Thompson to add pipes to Unix) and Donald Knuth wrote a word counting
program. Mcllroy used 6 lines of `sh`, while Knuth used 10 pages of
pascal.)
<https://stackoverflow.com/questions/25957835/wordcount-how-inefficient-is-mcilroys-solution>)

-- YOUR ANSWER HERE --

## What are some problems with Mcllroy's solution? How does MapReduce and other batch processing frameworks try to solve them?

-- YOUR ANSWER HERE --

## Read about a distributed filesystem (like GFS, Fossil, HDFS, etc) and explain it in your own words. How does it deal with distribution? What are some unique problems that distributed filesystems must be equipped to face?

-- YOUR ANSWER HERE --

## How are joins handled in MapReduce? What are some cons of using a hash of a key to determine where jobs are sent? What are some ways that can alleviate this strain?

-- YOUR ANSWER HERE --

## What is the difference between Map-Side joins and Reduce-Side joins? When would you choose one or the other? Why?

-- YOUR ANSWER HERE --

## What is MapReduce used for? Give an example of a time you needed a batch processing job to do work for you, and explain how you did it, and what tradeoffs that entailed.

-- YOUR ANSWER HERE --

## The Unix Philosophy and MapReduce emphasize immutability (you can redirect stdout to a file and try again in unix, and write to a file in MapReduce). What are some pros to this system? What are some cons you can think of?

-- YOUR ANSWER HERE --

## Why is MapReduce designed to handle faults so often? Is it a realistic assumption that most jobs on computers should fail?

-- YOUR ANSWER HERE --

## Explain how Batch processing can be used on graphs. What would be an application of a batch processing job on a graph? Likewise, what are some things to be careful about when operating a batch processing job on a graph?

-- YOUR ANSWER HERE --
