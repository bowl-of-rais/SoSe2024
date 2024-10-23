# Finding slot

| Approach                  | Cycles | Notes                      |
| ------------------------- | ------ | -------------------------- |
| `hash % hts`              | 100    | just don't                 |
| `mod` with magic number   | 6      | pre-computed magic numbers |
| `hash >> (64 - log2 hts)` | 1      | needs strong hash function |
| `(hash * hts) >> 64`      | 2      | uses `long` multiplication |
# Hash functions

| Approach          | Notes                  |
| ----------------- | ---------------------- |
| identity          | bad bad evil           |
| `sha`             |                        |
| `K * c`           | e.g. fibonacci hashing |
| `H *= odd_number` |                        |
| `H ^= h >> c`     |                        |
| `H += c`          |                        |
| `H ^= c`          |                        |

desiderata
- invertible (else we lose info and some parts of table become unreachable)

primitives combined:
- [xxHash](https://github.com/Cyan4973/xxHash)
- murmur hash
##### Other Attractive^TM options. non-portable

- CRC32
- AES: on CPU