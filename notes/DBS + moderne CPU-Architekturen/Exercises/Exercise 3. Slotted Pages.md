3 weeks UwU

# General

- dummy buffermanager
- check `schema.h` for useful functions
- header files in `include/moderndbs/`
- many sedge cases :c
- "be happy if pipeline is green"
- hexdump function for debugging


# `fsi_segment.cc`

[[1.2 Access Paths#Free Space Management]]

>[!imp] bound number of redirects to 1

>[!imp] store in BM, span multiple pages

- either use linear scale (easier one to start with) or hybrid or


# `slotted_page.cc`, `sp_segment.cc`

[[1.2 Access Paths#Slotted Pages]]

### `SlottedPage`

- `is_redirect()` and `is_redirect_target()`: slide Slotted Pages (7)
- `get_fragmented_free_space()`: how much
- `allocate` returns pos in page
- `compactify()`: 

----

# Working notes

#### Old encoding

```cpp
uint8_t FSISegment::encode_free_space(uint32_t free_space) {  
   // DONE: add your implementation here  
   return (free_space >> (28 - std::countl_zero(buffer_manager.get_page_size()))) ^ 0b1111; 
}  
  
uint32_t FSISegment::decode_free_space(uint8_t free_space) {  
   return (free_space ^ 0b1111) << (28 - std::countl_zero(buffer_manager.get_page_size())); 
}
```