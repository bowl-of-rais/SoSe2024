> build B-Tree from large amount of data

basic idea: sort before inserting -> locality :3

## Building from scratch

1. sort data
2. make leaf pages from data
	- save largest value in page in another file -> separators
3. make inner pages from separators
4. repeat until root

-----
## Bulk update

1. sort data
2. merge into existing tree
3. form pages, remember separators
4. new chunk when page would contain only entries from original trees
5. merge in separators

------

## Partitioned B-tree

> make bulk operations less disruptive

| Advantages                               | Disadvantages              |
| ---------------------------------------- | -------------------------- |
| independent partitions                   | no global order            |
| index always stays online                | lookups need access to all |
| lazy merge possible/only when beneficial | deletion is non-trivial    |