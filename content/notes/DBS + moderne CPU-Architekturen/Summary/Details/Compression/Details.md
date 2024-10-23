## Variable length encoding

![[Pasted image 20240724180013.png]]
-> either 0/1/2/4 or 1/2/3/4

- lookup location depends on preceding compression -> use lookup

## Dictionary compression

![[Pasted image 20240724180204.png]]

- store string ids in tuples

## Fast Static Symbol Table

> find common substrings

- random access and search on uncompressed data possible