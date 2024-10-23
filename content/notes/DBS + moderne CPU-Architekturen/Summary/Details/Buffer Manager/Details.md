## Buffer Frames

> maintain state of a certain page within the buffer

aka represents page while it's in memory

contained info:
- page number
- r/w latch
- LSN for recovery
- state
- data

----

## Replacement strategies

**FIFO**
- linked list of buffer frames
- no locality

**LRU**
- double-linked list
- remove from head
- unfix moves frame to end of list
- popular :3

**LFU**
- priority queue based on number of accesses
- expensive :c

**Second Chance**
- set bit when unfixing
- *clock*: unset bit while looking for pages to replace

**2Q**
- FIFO + LRU

> DBMS knows when it's scanning (= doing a read-once access), so it could give hints to BM when unfixing (`will-need`, `will-not-need`)