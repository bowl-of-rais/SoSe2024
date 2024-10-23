# Basics

### Actions & transactions

**action**: read, write, syscall
**transaction**: mutliple databases working together

>[!info] **ACID**

### Locks vs Latches

**locks**: for long-running user transactions

### Serial & serializable schedules

- serial: correct by definition (1 lock on whole database)
- serializable: concurrent but equivalent in terms of write-read relationships

**read committed**: permits inconsistent scans
**repeatable read**: preserve presence of existing values

**repeatable count**: count should have same result
- phantom: record that "sneaks in"
- phantom preservation

### Shared & exclusive locks

**intentional locking**: (vs absolute locking)

[Multiple granularity locking](https://en.wikipedia.org/wiki/Multiple_granularity_locking)

### Conflict probability and frequency

| Independent of               | depends on            |
| ---------------------------- | --------------------- |
| optimistic vs pessimistic cc | workload              |
|                              | granularity           |
|                              | phantom protection    |
|                              | premature publication |
|                              | version support       |

# Optimistic Concurrency Control

| Phase              | Notes                       | Traditional | Optimistic w/ backward validation                      |
| ------------------ | --------------------------- | ----------- | ------------------------------------------------------ |
| read               |                             |             |                                                        |
| commit preparation | validation                  | not needed  | check whether updates were valid                       |
| commit log record  |                             |             |                                                        |
| update propagation |                             | not needed  | updates can now be written                             |
| hardening          | write log to stable storage | last step   |                                                        |
| after completion   |                             |             | compare/check whether other transactions had conflicts |

# Multi-version storage

- read-only transactions: snapshot isolation, no locks
	- reads most recent version committed before start of transaction
- read-write transaction:
	- shared lock: read most recent committed versions
	- exclusive lock: create/modify/read an uncommitted version
		- can only be committed once no shared locks


# Lock durations
### Controlled lock violation

##### Exclusion effect

- no conflicts during hardening

### Deferred lock enforcement

- no conflicts during transaction work phase except write-write

>[!int] catches all true conflicts, avoids many false conflicts -> higher throughput

##### Remaining restrictions for read-write transactions

- no multiple concurrent writers: one would need to abort
- writer cannot commit with concurrent active reader

### Comparison

DLE/CLV/multi-version storage < timestamp validation < backward validation < traditional locking
- in terms of conflicts

# Lock sizes

[ARIES/KVL](https://www.vldb.org/conf/1990/P392.PDF)
- gaps between entries in sorted order
- presence and absence of entries can be locked
	- presence: lock entries + gaps between + before/after
	- absence: lock corresponding gap -> phantom protection

alternative: index management
- has a next key?
- data-only, index-specific
- locking logical row may affect other indexes

solution: key-range locking

### Orthogonal key-range locking

- 2 lock modes: index entry + gap
- none/shared/exclusive -> 3 x 3 = 9 values

### Partitioning instances

- partition row identifiers
- separate lock modes for each partition
- kind of granularity
- implemented with mod or some other bucketing/hashing method?
- also possible for gaps -> use ghost records
	- easy insertion by flagging record as valid

# Lock modes

## Increment/decrement locks


# Deadlocks

## lock acquisition sequence

