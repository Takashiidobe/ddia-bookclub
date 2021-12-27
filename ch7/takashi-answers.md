# Chapter 7 Questions

## Why do we need transactions? What are some of the ways in which a read or write to a database can fail?

A transaction is a mechanism to group several reads and writes together
into a logical unit.

This means that the database can take care of several error situations
such as partial writes succeeding or data corruption, which would be
difficult to deal with on the application side.

In typical concurrent programming, writers should have exclusive locks
(there can only be one writer and nothing else) and readers should grab
a reader lock (there can be as many readers as needed, but read locks
block exclusive locks). (This can be sidestepped with MVCC, but at the
cost of performance and complexity).

If these rules are violated, then data can be corrupted at any time,
which would be bad for application programmers.

## In the context of ACID, what does Atomicity (A) mean?

Atomicity means that for a client that tries to make a transaction, if
there is any fault, the database aborts the transaction and the database
discards any writes made in the transaction.

## In the context of ACID, what does Consistency (C) mean?

Consistency means that the database keeps certain invariants about the
system. This is generally an application property, and not something the
database enforces, outside of foreign key constraints, and primary key
violations.

## In the context of ACID, what does Isolation (I) mean?

Isolation means concurrent transactions do not affect each other.
Isolation is close to serializable in meaning, but with a heavy
performance penalty, which is why it's not often used in practice.

## In the context of ACID, what does Durability (D) mean?

Durability means that the database will not lose a transaction that it
has acknowledged. In this case, transactions have to committed to
non-volatile storage in some form, like an SSD.

## What are some of the ways in which hard drives can fail? Kleppmann notes that SSDs have more failure modes than HDDs, but that SSDs fail completely less than HDDs. Read [Backblaze's Hard Drive Failure rates for Q1](https://www.backblaze.com/blog/backblaze-hard-drive-stats-q1-2021/), written by a company that deals in cloud storage. Does the data support Kleppmann's notes? What are some of the ways in which Backblaze's analysis is lacking? What parts were illuminating?

1. The data does seem to correlate with what Kleppman is saying, noting
   that SSDs have a failure rate of about 5x of HDDs.

There are many ways that hard disks can have corrupted blocks (fail in
some way):

1. If your Hard drive doesn't have ECC, the suns rays can flip bits,
   corrupting data
2. Any machines connecting to data can die, effectively rendering the
   disk unreachable
3. A correlated fault can knock out all hard disks that are storing
   data, resulting in data loss for a segment of the application
4. SSDs have complicated firmware: Backblaze notes that some brands have
   better firmware than others, which leads to less block failure for
   certain brands, even with excessive hardware failures. Google is
   famous for writing software that promotes redundancy for their
   massive scale, since they worked with commodity hardware which lacked
   fast writes or even ECC
5. SSDs can move around faulty blocks and recognize when to move around
   certain blocks on disk to increase longevity, something that HDDs do
   not commonly implement
6. Fsync is fraught with danger. Writes that were thought to be flushed
   to the disk can be cached by the OS, which could crash before
   persisting to disk.

## What is a dirty read? What are some of its consequences?

A dirty read involves reading a transaction's uncommitted writes. This
means that the reader who reads the dirty information may provide
incorrect information to the client (the other transaction could be
aborted, which would make the information stale) or not all parts of the
transaction would have updated, in which case the user might receive
incorrect information.

## What are some levels of Transaction Isolation that databases provide? Note that Kleppmann does not go over Read Uncommitted, an ANSI SQL Transaction Isolation level, because it allows dirty reads (transactions can see uncommitted data in other transactions) which violates the point of transactions. Explain in your own words the Transaction Isolation levels, what they prevent, and what their performance costs are: (Bonus: How is each level implemented under the hood?) Postgres' Transaction Isolation level documentation here: [Transaction Isolation in Postgres](https://www.postgresql.org/docs/current/transaction-iso.html).

### Read Uncommitted

Postgres, like many other databases, does not implement Read
Uncommitted, because it allows for dirty reads of transactions. This has
the best performance but the worst consistency.

### Read Committed

Read committed is the default level. It makes it so concurrent
transactions can only see tuples at the state when they start the
transaction.

This is good enough for most transactions, but can cause causes where
the first write wins:

Imagine this transaction:

```sql
BEGIN;
UPDATE website SET hits = hits + 1;
-- from another session, UPDATE website SET hits = hits + 1;
COMMIT;
```

This will only update the tuple once, even though two concurrent
transactions were issued, because the two concurrent transactions set
hits to hits + 1, instead of hits + 2.

However, this does not stop state from changing under you during the
transaction:

Imagine this transaction:

```sql
BEGIN;
SELECT * from table;
WAITFOR DELAY '00:01:00' -- wait for a minute
SELECT * from table; -- any updates by other transactions are shown
COMMIT;
```

In Read Committed, the second select may return _any_ data. A concurrent
transaction may update the record, delete it, insert new records, etc.

This can be undesirable in some cases, so you'll need to use a higher
isolation level.

### Repeatable Read

Repeatable read is normally implemented using Snapshot isolation, so
these terms might be interchangeable.

Imagine this transaction:

```sql
BEGIN;
SELECT * from table;
WAITFOR DELAY '00:01:00' -- wait for a minute
SELECT * from table; -- this will show at least the rows added by a
concurrent transaction in that one minute, but existing rows cannot be
deleted nor changed.
COMMIT;
```

Under repeatable read, the same rows that the first select saw will be
displayed unchanged, but other items might be added to the second
select.

This is useful because you can imagine this transaction:

```sql
BEGIN;
-- Make sure there are at least 5 hits on the website
do $$
declare
  hits := SELECT hits FROM table where id = 1;
begin
  if hits > 5 then
    UPDATE hits = hits + 1 FROM table where id = 1; -- add a hit
  end if;
end $$;
COMMIT;
```

In Read Committed, the hits variable might have changed since it was
read in the declare block, and so the `UPDATE` can fail under Read
Committed.

In Snapshot isolation, each transaction gets a "snapshot" of global
state, which it can then commit to. This gives most of the benefits of
Serializable without its abysmal costs.

### Serializable

In serializable, in this transaction, the second select here will only
see the rows the first select saw:

```sql
BEGIN;
SELECT * from table;
WAITFOR DELAY '00:01:00' -- wait for a minute
SELECT * from table; -- this will show only the rows the first select
saw
COMMIT;
```

This blocks every other transaction, but creates a total ordering of the
database. Think of it as if the database is running in single threaded
mode. This is extremely straightforward for the application developer, but terrible for performance, because every transaction must be processed serially.

## Implementing Transactions in Parallel is extremely tedious and error prone. What happens if we execute transactions one by one? What are some of the ways that can fail?

We can execute transactions one by one, but this makes the database
extremely slow (it has a total ordering, but now any transaction blocks
all other transactions). If a transaction takes a long time or fails,
all other transactions behind it in the queue can fall behind, so
serializable/implementing transactions one by one can cause lots of
issues.

If we issue transactions in parallel, we can avoid performance problems,
but at the cost of CPU expenditure (Snapshot isolation) or phantom reads
(for read committed).

## How does MVCC (Multiversion concurrency control) work? What problems does it solve? In postgres, each transaction has a 32-bit txid. What can go wrong? (Hint: what happens if transaction ids overflow without being garbage collected?) Read this for a quick summary: [Postgres MVCC](https://malisper.me/postgres-mvcc/).

MVCC solves the problem of having either totally ordered transactions
(serializable) but still having transactions see a consistent view of
the database while in a transaction (by taking a snapshot of the
database at that point in time).

In postgres, each transaction has a 32-bit txid that is ever increasing
and must be garbage collected. In the case that too many transactions
occur and this number overflows, however, all transactions will fail
until this problem is dealt with.

## What is Two-Phase locking (2PL)? What are its pros and cons? What are some things we need to be careful about with this level of Transaction Isolation?

2PL is how the serializable isolation level is implemented by databases.
This can be promptly summarized by "Writers block readers and vice
versa". This causes poor performance for highly contentious rows (but
less CPU usage than Snapshot isolation), since if rows are not
concurrently accessed, this is roughly as performant as Read Committed.

In 2PL, since readers block writers and vice-versa, all transactions
must complete in a reasonable time period, otherwise the database will
be overwhelmed and reads and writes will start failing. Otherwise,
though, all events are totally ordered, so this is straightforward for
the application writer to use.

## Explain Serializable Snapshot Isolation (SSI) in your own words. What are some of its advantages to MVCC? What are some of its disadvantages?

2PL implements the serializable isolation level by being pessimistic,
i.e. by being correct beforehand, preserving invariants. Unfortunately,
that means that every row needs a lock where readers and writers block
each other, and acquiring and releasing locks is extremely expensive.
SSI is optimistic: it allows for transactions to proceed even if they
might update the same rows, and only commits transactions where things
succeeded as expected. Any corrupted transactions are aborted and
retried.

This is much more efficient in a practical level: there are no locks on
tables, but this puts more difficulty on the implementation, since
lockless programming is difficult. As well, transaction throughput can
become slow if there is high contention on a row.

## What is an advancement in databases that you are excited about? Give a quick summary of it and talk about the problems it solves.

An advancement I'm excited about in Databases is SSI (clojure implements
this for its atoms) and CAS (compare and set) operations allowing for
faster updates (lockless programming increases throughput of all kinds
of transactions).

On the application side, I'm looking forward to having distributed
relational databases by default, like cockroach DB or noria, a MySQL
compliant database that has a heavy caching layer for faster reads on
highly read data.

## Reflect on your own personal experience: Have you been bit by the rough edges or performance cliffs inherent in transactions? If so, give a quick summary of the problem you encountered and a possible solution. If you have not experienced a problem with transactions, take one of the transaction levels/algorithms covered in this chapter and explain a way to mitigate its performance/correctness costs at the application level.

I have personally been bit by MySQL's transaction timeouts: for rows
with high contention under repeatable read, transactions can timeout,
which causes writes to fail, and even worse, be queued for a long time
and then fail, which requires the application to requeue up for a write.
This required moving to zookeeper locks instead of MySQL locks for
scaling purposes.
