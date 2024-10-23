### Hashtables

- what is the read latency of main memory (roughly)?
	- latency number list: https://gist.github.com/jboner/2841832
	- 100ns
- what hashtable implementation did you fix in the debugging assignment?
	- open hashtable
- what hash function did the debugging assignment use?
	- murmurhash
- how many cache misses do you get per lookup for a large hash table with chaining and low fill factor?
	- few
- how many cache misses do you get per lookup for a large hash table with direct addressing and low fill factor?
	- also few

### B-Trees + Slotted Pages

- what is the worst case lookup performance in robin hood hashing?
	- $n$ aka number of entries
- what is the worst case lookup performance in cuckoo hashing?
	- $\mathcal{O}(1)$ because there \
- in which scenario can binary search be slower than linear search?
- why is a B-Tree usually faster than a binary tree?
- which instruction computes fast log2 on unsigned integers? `__builtin_clz()`, `popcount()` or `__builtin_ctz()`?
- how many comparisons does a lookup require on a B-Tree with N entries?
- how deep is a B-Tree with node size b and N entries?
- what is the main difference between a B+-Tree and a classical B-Tree?
- what data structure is a probabilistic alternative to a binary tree?
- how many comparisons does a lookup require on a B-Tree with B-sized nodes and N entries?
- what kind of dependencies prevent CPUs from pipelining program execution?
- how large is one cache line typically (bytes)?
- what is a typical node size (bytes) for a B-Tree? what are the trade-offs?
- how can you store variable-size entries in a B-Tree?
- what is a typical access latency (cycles) for an L1 cache?
- how can you handle recursive node splitting during a B-Tree insert?
- how can you speed up lookup in a B-Tree storing variable-size keys?

### ART + HOT

- how can you handle recursive node splitting during a B-Tree insert?
- list three optimizations for a B-Tree storing variable-size keys
- enumerate different names for tries
- what does the span parameter in a trie determine?
- how large are the nodes in a trie with span 5?
- what does the height of a trie depend on?
- what re-balancing algorithms for tries are there?
- what node sizes does the default adaptive radix tree (ART) have?
- what is the provable worst-case amount of bytes per key in an ART?
- what ART node types are there?
- why do we need the minimum function in the ART source code?
- what does the ART do if a stored prefix is longer than 9 bytes?
- why do we need to flip the keyByte in ART Node16?
- which data structure does each HOT node encode?
- how many cache misses to we expect in a large hash table with low fill factor for a lookup?
- which data structure would you use to serve a read-mostly workload on URLs?


### Synchronization

- why do we need a cache coherency protocol?
- what cache coherency protocol did we discuss in the lecture?
- what does this acronym stand for?
- what guarantees does C++ give you when a data race occurs?
- what operations on std::atomic did we discuss?
- what are the default ordering guarantees for std::atomic
- how many bytes can be atomically handled on x86-64?
- what memory ordering does x86-64 use?
- what are read/write locks?
- do read/write locks allow for scalable data structures? why / why not?
- which performance counter might indicate cache-line ping pong?
- what locking algorithms did we discuss in the lecture?
- what different kinds of non-blocking algorithms are there?
- why do we need specialized memory reclamation techniques?
- what is the ABA problem?
- what additional data does optimistic lock coupling require?
- why is the B-Tree more difficult to synchronize than the ART?
- how can we synchronize structure modification operations on a B-Tree?
- with OLC, what issues might a reader encounter during reads in a node?
- what is the key insight of B-Link Trees?
- why are B-Link Trees much more important when storing pages on disk?
- what 2 key ideas make the Bw-Tree possible?
- describe how deltas in the Bw-Tree mapping table are merged
- why do we index keys with the bit-wise reverse hash in the split ordered list?
- why do we need sentinel nodes in the split ordered list?

### Persistence

- what storage technologies did we discuss?
- what does ’SSD’ stand for?
- what are the ’organizational’ components on a SSD?
- how many random reads can you do on a HDD per second (roughly)?
- how many random reads can you do on an SSD per second (roughly)?
- what is the read latency of an HDD (roughly)?
- what is the read latency of an SSD (roughly)?
- what is the read latency of main memory (roughly)?
- what is the main challenge in achieving crash-persistence on PMem?

### Repetition

- what is a usual maximum fill factor for a (open/chaining) hash table?
- how many cache misses can you expect in a large (open/chaining) hash table for one lookup?
- what effect causes exponential probe sequence length growth in open addressing?
- what can cause a displacement in cuckoo hashing to fail?
- what is the average / worst-case performance of open addressing / chaining?
- why is a B-Tree usually faster than a binary tree?
- how deep is a B-Tree with node size b and N entries?
- why do we need to store the keys outside of ART (database) as well?
- what three parameters n, m and k determine the false positive rate of a bloom filter?
- describe workloads where a B-Tree / ART / Bw-Tree might be faster
- describe a use-case where chaining might be faster than open addressing
- which C++ memory orders are there? Which one can I use for unlock to be faster on x86?
- how can I implement a lock-free singly-linked list?
- what does the span parameter in a trie determine?
- I insert 10 8 Byte keys into an ART / Radix Tree. What’s the best/worst-case height?