## Page structure

**inner node**:
- LSN
- upper
- count
- keys, children

**leaf node**:
- LSN
- marker for leaf node
- next
- count
- keys, values

----

## Lookup

1. start with root
2. if leaf: locate entry in current page
3. binary search for first >= key to find next page
4. go to 2.

-----

## Insert

1. find correct leaf
2. split if there isn't enough space (naive variant: propagates back to parents)
3. insert in leaf

----

## Delete

1. find leaf; erase entry
2. if not half-full: 
	- if neighbor more than half full: balance + update separator
	- else: merge with neighbor + remove separator + go to 2.

> logic can be simplified by accepting underfull pages

-----

## Range Scan

1. lookup start
2. find subsequent entries with `next` pointer