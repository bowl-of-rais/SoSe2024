main challenge: pages depend upon eachother

## Lock coupling

> latch parent + current node/page

- prevents conflicts during splitting
- ordered latching, so no dealocks

## Inserts

> propagating splits upwards could lead to deadlocks -> avoid!

**safe inner pages**/**greedy splitting**: split full inner pages while traversing tree

**restart**: keep all latches in second try -> simpler implementation, esp. for variable-length keys

## B-link trees

> a way to lock only 1 page at a time?

- add `next` pointers to inner nodes
- allows to check for meantime splits
- makes `delete` more complicated