## B-Tree-like

> relative offsets as search keys

- flexible, but maybe overkill?
- not in SQL

## Extent list

header: hash + length
body: page + extent (# of subsequent pages)

- changes only in 1 piece, but usually good enough

## Optimizing

1. BLOB < TID: encode in TID
2. BLOB < page: record
3. larger BLOBs use full mechanism