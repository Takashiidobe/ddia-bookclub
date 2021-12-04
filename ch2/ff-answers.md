# Chapter 2 Questions

## Explain NoSQL and Relational Databases in your own words. What are their strengths? What are their weaknesses?

####Relational
Relational databases are a data model implementation in which data relationships are organized into collections, typically known as tables and rows in SQL. This model was derived from needs in the business world, requiring relatively consistent structure across different use cases, and its underlying goal was to abstract away the underlying implementation of the database for developers. 

#####Strengths
-Access to data is straightforward due to its format
-Query optimization (ideal query execution) rarely required by developer
-New querying methods and features easy to implement
-Query optimizers, written once, can improve the experience of accessing the DB for all applications that come after

#####Weaknesses
-Query optimizers for RDBs require significant time and resources to develop
-Model rarely reflects format of the data on application side

####NoSQL
NoSQL is one of the few competing models to relational that has seen successful adoption. Rather than organize data into tables and rows, NoSQL's document model simply organizes data in an unstructured manner, allowing for multiple data representations within the same database.

#####Strengths
-Easier scalability features
-Better alignment and reception by FOSS
-Unique querying operations not always found in RDBs
-Relatively greater flexibility and extensibility

#####Weaknesses
-Difficult to handle many-to-many relationships (i.e. joins), pushing such logic into application code
-Cannot refer to nested items within documents
-The larger the document in the database, the more expensive it is to load smaller portions of it

## There are three types of NoSQL databases (KV, Document, Graph). Explain each in your own words and some of the strengths and weaknesses of each.

####Key-Value

#####Strengths
-
#####Weaknesses
-
####Document

#####Strengths
-
#####Weaknesses
-
####Graph

#####Strengths
-
#####Weaknesses
-

## What was your favorite query language this chapter? Why? Explore a query language you haven't tried before and try to explain its pros and cons.

-- YOUR ANSWER HERE --

## Assume your next project is to create a Social Network. How would you store the data, and why?

-- YOUR ANSWER HERE --

## Mapreduce is the first example of a distributed batch processing paradigm. Give a short summary of it. How would you improve it? What does it do well? What are some things you dislike about it?

-- YOUR ANSWER HERE --

## It is said that NoSQL databases do not require a schema up front. Is this true? Conversely, is it possible to change the schemas of SQL databases after creation? If so, what are the caveats?

-- YOUR ANSWER HERE --
