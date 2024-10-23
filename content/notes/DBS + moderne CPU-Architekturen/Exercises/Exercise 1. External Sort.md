- [x] setup for exercise ⛔ 51l30n 📅 2024-04-30 ✅ 2024-04-17
- [x] do assignment ⛔ 51l30n 📅 2024-04-30 ✅ 2024-05-08

---

# Debrief

- discussed on 07.05.


-----

based on the [[Motivational Example]] from the lecture

---

# Links




---
# Bugs

![[Pasted image 20240421000718.png]]
which is a shift by 1 byte?
![[Pasted image 20240421000809.png]]
![[Pasted image 20240421000756.png]]
sooo an off by 1 error somewhere?

output:
```
Which is: { 
188752050786048, 188752050786048,  // 14x
188752050786048, 188752050786048, 
188752050786048, 188752050786048, 
188752050786048, 188752050786048, 
188752050786048, 188752050786048, 
188752050786048, 188752050786048, 
188752050786048, 188752050786048, 
737312698383, 737312698383,        // 14x
737312698383, 737312698383, 
737312698383, 737312698383, 
737312698383, 737312698383, 
737312698383, 737312698383, 
737312698383, 737312698383, 
737312698383, 737312698383, 
188752050786048, 188752050786048, 
188752050786048, 188752050786048, ... }
```


----
# Scribbles

#### k-way

###### Attempt 1

```cpp nums
// MERGE

size_t k = num_values * sizeof(uint64_t) / mem_size;  
size_t buffer_size = mem_size / (k+1);     // what if mem_size is too small even for this?  
  
std::vector<std::vector<uint64_t>> input_buffers;  
std::vector<uint64_t> output_buffer(buffer_size);


// initialize buffers for merge  
for (size_t i = 0; i < k; i++) {  
    in_char_buffer = tmp->read_block(i * mem_size, buffer_size);  
    uint_buffer = std::launder(reinterpret_cast<uint64_t*>(in_char_buffer.get()));  
    input_buffers.emplace_back(uint_buffer, uint_buffer + buffer_size/sizeof(uint64_t));  
    std::make_heap(input_buffers.back().begin(), input_buffers.back().end(), std::greater<>());  
}
```

##### Attempt 2

```cpp
// part 2: merge  
  
const size_t nums_in_mem = mem_size / sizeof (uint64_t);  
  
size_t k = num_values / nums_in_mem; // num_values * sizeof(uint64_t) / mem_size  
size_t buffer_size = mem_size / (k+1);     // what if mem_size is too small even for this?  
  
if (buffer_size < 8) {  
   return;  
}  
  
std::vector<std::vector<uint64_t>> input_buffers;  
std::vector<uint64_t> output_buffer{};  
offset = 0;  
std::vector<size_t> merge_offsets{};  
  
// initialize buffers for merge -> k heaps  
for (size_t i = 0; i < k; i++) {  
    in_char_buffer = tmp->read_block(i * mem_size, buffer_size);  
    uint_buffer = std::launder(reinterpret_cast<uint64_t*>(in_char_buffer.get()));  
    input_buffers.emplace_back(uint_buffer, uint_buffer + buffer_size/sizeof(uint64_t));  
    std::make_heap(input_buffers.back().begin(), input_buffers.back().end(), std::greater<>());  
    merge_offsets.emplace_back(buffer_size/sizeof(uint64_t));  
}  
  
for (size_t i = 0; i < num_values; i++) {  
    // find smallest first element  
    size_t argmin = 0;  
    for (size_t j = 0; j < k; j++){  
        if (input_buffers.at(j).back() < input_buffers.at(argmin).back()) {  
            argmin = j;  
        }  
    }  
   uint64_t min = input_buffers.at(argmin).back();  
   input_buffers.at(argmin).pop_back();  
  
    if (input_buffers.at(argmin).empty()) {  
        // refill  
        in_char_buffer = tmp->read_block(argmin * mem_size + merge_offsets.at(argmin), buffer_size);  
        uint_buffer = std::launder(reinterpret_cast<uint64_t*>(in_char_buffer.get()));  
        input_buffers.at(argmin) = std::vector(uint_buffer, uint_buffer + buffer_size/sizeof(uint64_t));  
        std::make_heap(input_buffers.back().begin(), input_buffers.back().end(), std::greater<>());  
        merge_offsets.at(argmin) += buffer_size/sizeof(uint64_t);  
    }  
  
    // write to output buffer  
    output_buffer.push_back(min);  
  
    if (output_buffer.size() == buffer_size) {  
        out_char_buffer = std::launder(reinterpret_cast<const char*>(output_buffer.data()));  
        output.write_block(out_char_buffer, offset, buffer_size);  
        output_buffer.clear();  
        offset += buffer_size;  
    }  
}
```

**idea**: when chunk sorted is fully merged, use the space (give it to other chunks)?

```cpp
// part 2: merge  
  
const size_t nums_in_mem = mem_size / sizeof (uint64_t);  
  
size_t k = num_values / nums_in_mem + (num_values % nums_in_mem != 0);; // num_values * sizeof(uint64_t) / mem_size  
size_t max_buffer_size = mem_size / (k+1);     // what if mem_size is too small even for this?  
  
if (max_buffer_size < 8) {  
    std::cout << "max_buffer_size too small!";  
    return;  
}  
  
//std::vector<std::vector<uint64_t>> sorted_chunks;  
std::vector<std::priority_queue<uint64_t, std::vector<uint64_t>, std::greater<>>> sorted_chunks;  
std::vector<uint64_t> merged{};  
size_t write_offset = 0;  
std::vector<size_t> read_offsets{};  
  
// initialize buffers for merge -> k heaps  
for (size_t i = 0; i < k; i++) {  
    to_read = std::min(max_buffer_size, tmp->size());  
    //sorted_chunks.emplace_back(read_ints(i*mem_size, to_read, *tmp));  
    //std::make_heap(sorted_chunks.back().begin(), sorted_chunks.back().end(), std::greater<>());    //std::pop_heap(sorted_chunks.back().begin(), sorted_chunks.back().end(), std::greater<>());    std::vector<uint64_t> chunk = read_ints(i*mem_size, to_read, *tmp);  
    sorted_chunks.emplace_back(chunk.begin(), chunk.end(), std::greater<>());  
    read_offsets.emplace_back(to_read);  
}  
  
for (size_t i = 0; i < num_values; i++) {  
    // find smallest first element  
    size_t argmin = 0;  
    for (size_t j = 0; j < k; j++) {  
        if (sorted_chunks.at(argmin).empty() || sorted_chunks.at(j).top() < sorted_chunks.at(argmin).top()) {  
            argmin = j;  
        }  
    }  
    //uint64_t min = sorted_chunks.at(argmin).back();  
    //sorted_chunks.at(argmin).pop_back();    //std::pop_heap(sorted_chunks.at(argmin).begin(), sorted_chunks.at(argmin).end());  
    uint64_t min = sorted_chunks.at(argmin).top();  
    sorted_chunks.at(argmin).pop();  
  
    if (sorted_chunks.at(argmin).empty()) {  
        // refill  
        to_read = std::min(max_buffer_size, tmp->size() - (argmin*mem_size + read_offsets.at(argmin)));  
        if (to_read > 0 && read_offsets.at(argmin) < mem_size) {  
            //sorted_chunks.at(argmin).clear();  
            //sorted_chunks.at(argmin) = read_ints(argmin * mem_size + read_offsets.at(argmin), to_read, *tmp);            // std::make_heap(sorted_chunks.at(argmin).begin(), sorted_chunks.at(argmin).end(), std::greater<>());            // std::pop_heap(sorted_chunks.at(argmin).begin(), sorted_chunks.at(argmin).end(), std::greater<>());            std::vector<uint64_t> chunk = read_ints(i*mem_size, to_read, *tmp);  
            sorted_chunks.at(argmin) = std::priority_queue(chunk.begin(), chunk.end(), std::greater<>());  
            read_offsets.at(argmin) += to_read;  
        }  
    }  
  
    // write to output buffer  
    merged.push_back(min);  
  
    if (merged.size() == max_buffer_size || i == num_values - 1) {  
        size_t to_write = merged.size() * sizeof(uint64_t);  
        write_ints(merged, write_offset, to_write, output);  
        merged.clear();  
        write_offset += to_write;  
    }  
}
```

```cpp
std::vector<uint64_t> read_ints(size_t offset, size_t size, File& file) {  
    std::unique_ptr<char[]> raw = file.read_block(offset, size);  
    uint64_t* casted = std::launder(reinterpret_cast<uint64_t*>(raw.get()));  
    std::vector<uint64_t> numbers(casted, casted + size/sizeof(uint64_t)); // TODO: copies the whole thing :c  
    return numbers;  
}  
  
void write_ints(std::vector<uint64_t> numbers, size_t offset, size_t size, File& file) {  
    auto raw = std::launder(reinterpret_cast<const char*>(numbers.data()));  
    file.write_block(raw, offset, size);  
}  
  
void external_sort(File& input, size_t num_values, File& output, size_t mem_size) {  
    // part 1: sort max mem_sized chunks. do 1 pass over input file  
  
    if (input.size() % 8 != 0) {  
        throw std::invalid_argument("Invalid input file, should contain uint64");  
    }  
  
    size_t read_offset = 0;  
    size_t to_read = std::min(input.size(), mem_size);  
  
    std::unique_ptr<File> tmp = File::make_temporary_file();  
    tmp->resize(input.size());  
    output.resize(input.size());  
  
    while (read_offset < input.size()) {  
        auto numbers = read_ints(read_offset, to_read, input);  
  
        std::sort(numbers.begin(), numbers.end());  
  
        write_ints(numbers, read_offset, to_read, *tmp);  
  
        read_offset += to_read;  
        to_read = std::min(input.size() - read_offset, mem_size);  
    }  
  
    // part 2: merge  
  
    const size_t nums_in_mem = mem_size / sizeof (uint64_t);  
    const size_t k = num_values / nums_in_mem + (num_values % nums_in_mem != 0);  
    const size_t max_buffer_size = ((mem_size / (k+1)) + 7) & -8;     // make this divisible by 8  
  
    std::cout << "mem_size: " << mem_size << "\n";  
    std::cout << "num_values: " << num_values << "\n";  
    std::cout << "k: " << k << "\n";  
    std::cout << "nums_in_mem: " << nums_in_mem << "\n";  
    std::cout << "max_buffer_size: " << max_buffer_size << " aka " << max_buffer_size/8 << " numbers\n\n";  
  
    if (max_buffer_size < 8) {  
        std::cout << "max_buffer_size too small!"; // TODO: deal with this case  
        return;  
    }  
  
    std::vector<std::priority_queue<uint64_t, std::vector<uint64_t>, std::greater<>>> sorted_chunks{};  
    std::vector<uint64_t> merged{};  
    size_t write_offset = 0;  
    std::vector<size_t> read_offsets{}; // for each chunk: how much is read/merged so far  
  
    // initialize buffers for merge -> k queues    for (size_t i = 0; i < k; i++) {  
        to_read = std::min(max_buffer_size, tmp->size() - i*mem_size);  
        std::vector<uint64_t> chunk = read_ints(i*mem_size, to_read, *tmp);  
        sorted_chunks.emplace_back(chunk.begin(), chunk.end(), std::greater<>());  
        read_offsets.emplace_back(to_read);  
    }  
  
    for (size_t i = 0; i < num_values; i++) {  
        // find smallest first element  
        size_t argmin = 0;  
        for (size_t j = 0; j < k; j++) {  
            std::cout << sorted_chunks.at(argmin).empty() << "\n";  
            if (sorted_chunks.at(argmin).empty() || (!sorted_chunks.at(j).empty() && sorted_chunks.at(j).top() < sorted_chunks.at(argmin).top())) {  
                argmin = j;  
            }  
        }  
        uint64_t min = sorted_chunks.at(argmin).top();  
        std::cout << min << " from chunk " << argmin << "\n";  
  
        sorted_chunks.at(argmin).pop();  
  
        if (sorted_chunks.at(argmin).empty()) {  
            // refill  
            std::cout << "queue empty at " << argmin << "\n";  
            size_t actual_offset = argmin*mem_size + read_offsets.at(argmin);  
            to_read = std::min(max_buffer_size, tmp->size() - actual_offset); // max buffer or everything left in file  
            to_read = std::min(to_read, mem_size - read_offsets.at(argmin)); // or everything left in chunk  
            if (to_read > 0 && read_offsets.at(argmin) < mem_size) {  
                std::cout << "refilling\n";  
                std::vector<uint64_t> chunk = read_ints(actual_offset, to_read, *tmp);  
                for (uint64_t n : chunk) { std::cout << n << " "; }  
                std::cout << "\n\n";  
                sorted_chunks.at(argmin) = std::priority_queue(chunk.begin(), chunk.end(), std::greater<>());  
                read_offsets.at(argmin) += to_read;  
            }  
        }  
  
        // write to output buffer  
        merged.push_back(min);  
  
        if (merged.size() == max_buffer_size / sizeof (uint64_t) || i == num_values - 1) {  
            std::cout << "writing " << merged.size() << "\n";  
            for (uint64_t n : merged) { std::cout << n << " "; }  
            std::cout << "\n\n";  
            size_t to_write = merged.size() * sizeof(uint64_t);  
            write_ints(merged, write_offset, to_write, output);  
            merged.clear();  
            write_offset += to_write;  
        }  
    }  
  
}
```


```cpp
std::vector<std::priority_queue<uint64_t, std::vector<uint64_t>, std::greater<>>> sorted_chunks{};  
std::vector<uint64_t> merged{};  
size_t write_offset = 0;  
std::vector<size_t> read_offsets{}; // for each chunk: how much is read/merged so far  
  
// initialize k queues for merge  
for (size_t i = 0; i < k; i++) {  
   nums_to_read = std::min(max_buffer_size, num_values - i * max_nums_in_mem);  
   std::vector<uint64_t> chunk = read_ints(i * max_nums_in_mem, nums_to_read, *tmp);  
   /* std::cout << "reading " << nums_to_read << "\n";  
   for (uint64_t n : chunk) { std::cout << n << " "; }   std::cout << "\n\n"; */   sorted_chunks.emplace_back(chunk.begin(), chunk.end(), std::greater<>());  
   read_offsets.emplace_back(nums_to_read);  
}  
  
for (size_t i = 0; i < num_values; i++) {  
   // find smallest first element  
   size_t argmin = 0;  
   for (size_t j = 0; j < k; j++) {  
      if (sorted_chunks.at(argmin).empty() ||  
          (!sorted_chunks.at(j).empty() && sorted_chunks.at(j).top() < sorted_chunks.at(argmin).top())) {  
         argmin = j;  
      }  
   }  
   uint64_t min = sorted_chunks.at(argmin).top();  
  
   sorted_chunks.at(argmin).pop();  
  
   if (sorted_chunks.at(argmin).empty()) {  
      // refill empty queue  
      size_t actual_offset = argmin * max_nums_in_mem + read_offsets.at(argmin);  
      nums_to_read = std::min(max_buffer_size, num_values - actual_offset); // max buffer or everything left in file  
      nums_to_read = std::min(nums_to_read, max_nums_in_mem - read_offsets.at(argmin)); // or everything left in chunk  
      if (nums_to_read > 0 && read_offsets.at(argmin) < max_nums_in_mem) {  
         std::vector<uint64_t> chunk = read_ints(actual_offset, nums_to_read, *tmp);  
         /*std::cout << "reading " << nums_to_read << "\n";  
         for (uint64_t n : chunk) { std::cout << n << " "; }         std::cout << "\n\n"; */         sorted_chunks.at(argmin) = std::priority_queue(chunk.begin(), chunk.end(), std::greater<>());  
         read_offsets.at(argmin) += nums_to_read;  
      }  
   }  
  
   // write to output buffer  
   merged.push_back(min);  
  
   if (merged.size() == max_buffer_size || i == num_values - 1) {  
      // write buffer to output file  
      /* std::cout << "writing " << merged.size() << "\n";      for (uint64_t n : merged) { std::cout << n << " "; }      std::cout << "\n\n"; */      size_t to_write = merged.size();  
      write_ints(merged, write_offset, output);  
      merged.clear();  
      write_offset += to_write;  
   }  
}
```


#### 2-way

```cpp
size_t merge_size = mem_size / (3);     // what if mem_size is too small even for this?  
  
unsigned long* uint_buffer_2; // TODO: better names  
std::vector<uint64_t> output_buffer(merge_size);  
  
  
// initialize buffers for merge  
in_char_buffer = tmp->read_block(0, merge_size);  
uint_buffer = std::launder(reinterpret_cast<uint64_t*>(in_char_buffer.get()));  
std::make_heap(uint_buffer, uint_buffer + merge_size / 8, std::greater<>());  
  
in_char_buffer = tmp->read_block(merge_size, merge_size);  
uint_buffer_2 = std::launder(reinterpret_cast<uint64_t*>(in_char_buffer.get()));  
std::make_heap(uint_buffer_2, uint_buffer_2 + merge_size/8, std::greater<>());
```