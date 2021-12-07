# Chapter 7 Questions

## Why do we need transactions? What are some of the ways in which a read or write to a database can fail?

-- YOUR ANSWER HERE --

## In the context of ACID, what does Atomicity (A) mean?

-- YOUR ANSWER HERE --

## In the context of ACID, what does Consistency (C) mean?

-- YOUR ANSWER HERE --

## In the context of ACID, what does Isolation (I) mean?

-- YOUR ANSWER HERE --

## In the context of ACID, what does Durability (D) mean?

-- YOUR ANSWER HERE --

## What are some of the ways in which hard drives can fail? Kleppmann notes that SSDs have more failure modes than HDDs, but that SSDs fail completely less than HDDs. Read [Backblaze's Hard Drive Failure rates for Q1](https://www.backblaze.com/blog/backblaze-hard-drive-stats-q1-2021/), written by a company that deals in cloud storage. Does the data support Kleppmann's notes? What are some of the ways in which Backblaze's analysis is lacking? What parts were illuminating?

-- YOUR ANSWER HERE --

## What is a dirty read? What are some of its consequences?

-- YOUR ANSWER HERE --

## What are some levels of Transaction Isolation that databases provide? Note that Kleppmann does not go over Read Uncommitted, an ANSI SQL Transaction Isolation level, because it allows dirty reads (transactions can see uncommitted data in other transactions) which violates the point of transactions. Explain in your own words the Transaction Isolation levels, what they prevent, and what their performance costs are: (Bonus: How is each level implemented under the hood?) Postgres' Transaction Isolation level documentation here: [Transaction Isolation in Postgres](https://www.postgresql.org/docs/current/transaction-iso.html).

### Read Uncommitted

-- YOUR ANSWER HERE --

### Read Committed

-- YOUR ANSWER HERE --

### Repeatable Read

-- YOUR ANSWER HERE --

### Serializable

-- YOUR ANSWER HERE --

## Implementing Transactions in Parallel is extremely tedious and error prone. What happens if we execute transactions one by one? What are some of the ways that can fail?

-- YOUR ANSWER HERE --

## How does MVCC (Multiversion concurrency control) work? What problems does it solve? In postgres, each transaction has a 32-bit txid. What can go wrong? (Hint: what happens if transaction ids overflow without being garbage collected?) Read this for a quick summary: [Postgres MVCC](https://malisper.me/postgres-mvcc/).

-- YOUR ANSWER HERE --

## What is Two-Phase locking (2PL)? What are its pros and cons? What are some things we need to be careful about with this level of Transaction Isolation?

-- YOUR ANSWER HERE --

## Explain Serializable Snapshot Isolation (SSI) in your own words. What are some of its advantages to MVCC? What are some of its disadvantages?

-- YOUR ANSWER HERE --

## What is an advancement in databases that you are excited about? Give a quick summary of it and talk about the problems it solves.

-- YOUR ANSWER HERE --

## Reflect on your own personal experience: Have you been bit by the rough edges or performance cliffs inherent in transactions? If so, give a quick summary of the problem you encountered and a possible solution. If you have not experienced a problem with transactions, take one of the transaction levels/algorithms covered in this chapter and explain a way to mitigate its performance/correctness costs at the application level.

-- YOUR ANSWER HERE --

