provided: `impl/node4.hpp`, `impl/node16.hpp` (SIMD)

to implement: `impl/node48.hpp`, `impl/node48.hpp`

tasks:
1. standART
2. node sizes 2, 8, 32, 64, 256
	- different variants/node layouts possible for each
	- put them wherever
	- specify base node in ART (4 or now 2)

---

comparability:
- binary is chopped up
- bytes should be comparable in a way that makes sense for the whole value

----

- constructors from other/smaller nodes in `art.hpp`

#### `node4.hpp`

- lookup: linear scan
- insert: 
	- `ArtValue& ref`: reference in parent to this node, to replace by `ArtNode16` if node grows

#### `node64.hpp`

- SIMD -> intel intrinsics guide

#### Lazy expansion

- marked Leaf - tagged pointers, bit set

>[!idea] expand until key differ -> think about suffix

#### Path compression

>[!idea] prepend prefixes to node 

- lookup - skip: works bc we know prefix length