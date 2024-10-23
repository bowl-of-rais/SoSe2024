### 1. Access

1. B-trees: what exactly does clustering mean in this context
	- basically inter-page/inter-node locality?
2. long records: offsets for BLOBs in B-Trees are relative to what?

#### 2. Algebraic Operators

1. query operators: how exactly are registers implemented?
	- in the exercise they were called registers but we still used references to the tuples, right?
	- is the difference that when there's a pipeline break/aggregate, the materialized tuples are also stored in some global space instead of within the operator?

#### 3. Main-Memory Databases

1. slide "implementing DB transactions with HTM"
	- what is TSO
	- what does this slide want to tell me
2. latching vs locking?

#### 4. Transactions/Recovery

1. TAs/Recovery: what exactly defines a consistent state?