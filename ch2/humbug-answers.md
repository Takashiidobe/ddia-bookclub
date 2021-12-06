# Chapter 2 Questions

## Explain NoSQL and Relational Databases in your own words. What are their strengths? What are their weaknesses?

Relational databases are generally made up of structured data (aka tables or
relations) so it tends to be more rigid while non-relational databases are more
dynamic, meaning there are less rules surrounding data-format. Non-relational
databases can be represented as a graph or a simple KV table.

Relational databases are 'safe' in that the format of the data you insert and
query are guaranteed. It's harder to horizontally scale relational databases
since a common query can fetch and process multiple tables and rows, so they
tend to be scaled up vertically. Additionally, they've been around for a while
and many of the existing relational database management systems are
ACID-compliant, meaning they meet some reliability guarantees.

Non-relational databases tend to be less focused on fetching and processing
multiple tables and rows, but more on horizontal scalability and performance.
Instead of having structured data, their data tends to be unstructured and they
leave the processing to the clients. They're newer and many NoSQL databases
don't fully support ACID the way that relational databases do, IIUC.

## There are three types of NoSQL databases (KV, Document, Graph). Explain each in your own words and some of the strengths and weaknesses of each.

I'm going to lump KV and document-based databases into the same family, as they
boil down to having some value (whether it's unstructured data or a document)
keyed by some unique id. Document-based databases can additionally support more
features that depend on the document itself (like indexing based on a field
within the document) since it's loosely-structured. These databases tend to
have less consistency guarantees, however they are very simple and easy to
understand (and also easy to horizontally scale).

Graph databases store their data in a graph, where both nodes and edges contain
data. The data in the edges link the data in the nodes together, so that they
can be traversed and queried together easily. These types of databases are good
for pieces of data that have many relations. Graph databases can be hard to
shard (and therefore hard to horizontally scale), since you'd need to ensure
that 'connected components' sit on the same shard. However, they are easy to
visualize and some of their query languages enable us to do things like
declaratively traverse all edges/vertices that contain X data.

## What was your favorite query language this chapter? Why? Explore a query language you haven't tried before and try to explain its pros and cons.

SPARQL is my favorite query language in this chapter -- it's a nice and concise
declarative language. I liked all the declarative query languages, but I picked
SPARQL because it looked more concise. Declarative languages give people who
work on the query-engine of these databases a lot of freedom and room for
optimizations.

I'll be exploring [PromQL](https://prometheus.io/docs/prometheus/latest/querying/basics/).

Off the bat, I think the query language isn't very uniform and it's pretty hard
to tell what's happening (under the hood) based on the query language. There's
no clear order in operations, at least to me
(see: `sum(http_requests_total{method="GET"} offset 5m @ 1609746000)`). It also
looks like it's hard to 'join' timeseries together using this query language
(I wasn't able to find something on their reference doc). OTOH, I guess this
makes this query language pretty straight-forward and easy to use since it's so
short and limited (despite the operations being all over place, IMO).

## Assume your next project is to create a Social Network. How would you store the data, and why?

I'd store the social network data as a graph (so in a graph database). Things
(places, events, etc.) and people would be stored as vertices in this database
and edges would be actions (likes, is from, went to, etc.). It just makes more
sense to store this as a graph instead of a relational database because of how
many relations exist in a Social Network. A declarative query language for one
of the graph databases would also make queries simpler, when compared to a SQL
query.

## Mapreduce is the first example of a distributed batch processing paradigm. Give a short summary of it. How would you improve it? What does it do well? What are some things you dislike about it?

MapReduce is a programming model where we can process data across many machines
by implementing a few custom stateless functions, namely map and reduce. It
gives clients a lot of control over how they want to transform and aggregate the
data than a regular query language and query engine would. By being stateless,
it is also somewhat limited in functionality. It could be improved by adding
more complex operations (like keeping track of some state shared between
map/reduce stages). There is bound to be a bottleneck since not all data across
the machines will be equal, so some type of query planning would also be
helpful to prevent bottlenecks (e.g. 1 machine has 1TiB data, 99 machines have
1GiB).

## It is said that NoSQL databases do not require a schema up front. Is this true? Conversely, is it possible to change the schemas of SQL databases after creation? If so, what are the caveats?

Since NoSQL is pretty ambiguous, it may not be true for some of the NoSQL
database systems out there. I could see a graph database having some sort
of schema required up-front.

Schema changes in SQL databases are possible, but should be introduced carefully
and slowly. Specifically: you'd need to be careful about interactions between
services and the database (backward and forward compatibility); you need to be
careful about not introducing latency or downtime to users of the database while
you're updating it (under a lock/transaction?); and you need to be careful that
the schema takes effect immediately (partial new schema, partial old schema).
I'm not super familiar with the last one, but I imagine most relational
database management systems can handle that natively?
