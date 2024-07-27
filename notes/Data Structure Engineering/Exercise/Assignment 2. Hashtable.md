implement chaining and open addressing hash tables

- for vector, try to avoid resize

perf thing outputs normalized by scale

---


# Print

```cpp
                std::cout << "table:\n";
                for (auto v: ht) {
                    std::cout << v << "\n";
                }
                std::cout << "memory:\n";
                for (size_t j = 0; j < memory.size(); j++) {
                    Entry* v = &memory.at(j);
                    std::cout << v << "(" << v->key << ", " << v->value << ", " << v->next << ")" << "\n";
                    if(v->next) {
                        std::cout << "        (" << v->next->key << ", " << v->next->value << ", " << v->next->next << ")" << "\n";
                    }
                }
                 
```