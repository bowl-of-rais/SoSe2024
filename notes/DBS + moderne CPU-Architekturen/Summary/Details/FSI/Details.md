## Encoding free space

> nibble indicates fill status (sometimes also just 1 or 2 bits)

**linear**: $\texttt{data size}/ \frac{\texttt{page size}}{2^{\texttt{bits}}}$

**logarithmic**: $\lceil \log_{2}(\texttt{data size}) \rceil$

common problems:
- loss of accuracy in lower range
- most pages will be static + full, growth mostly at end
	- solution: cache next matching page?
##### From exercise

```cpp
uint8_t FSISegment::encode_free_space(uint32_t free_space) {
   return free_space ? 32 - std::countl_zero(free_space) - 1 : 0;
}
uint32_t FSISegment::decode_free_space(uint8_t free_space) {
   return free_space ? (1ul << free_space) : 0;
}
```