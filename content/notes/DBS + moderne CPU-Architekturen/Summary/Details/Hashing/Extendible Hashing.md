>multiple entries point to same bucket

**depth** ~ how many bits of hash value used?

```mermaid
graph TD
	A(bucket overflow:
	time to split) --> B(maximum depth?)
	B -- no --> C(increase depth)
	C --- D([de-share:
	use one more bit])
	B --- yes ---> G(maximum table size?)
	G -- no --> E(double table)
	E --- F([new sharing:
	adds bit at end])
	G -- yes --> H(start chaining)
```

![[Pasted image 20240725192332.png]]

**load factor**: for balancing chaining vs growth

| Advantages                             | Disadvantages                    |
| -------------------------------------- | -------------------------------- |
| 2 page accesses per lookup             | invasive growth                  |
| growth independent of existing buckets | large steps in space consumption |
| no rehashing                           | collisions...?                   |
