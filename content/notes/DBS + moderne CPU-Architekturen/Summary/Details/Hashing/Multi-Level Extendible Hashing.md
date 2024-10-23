> real data is not uniform: poor space utilization

![[Pasted image 20240725201240.png]]

> tree of hash tables

naive approach: "next level uses next $k$ bits, 1 node per page"
- poor space utilization
- uniform data: simultaneous overflows

better: **share inner page between buddies**
- more bits when inner page is split
- buddies can be moved up again

![[Pasted image 20240725201940.png]]

