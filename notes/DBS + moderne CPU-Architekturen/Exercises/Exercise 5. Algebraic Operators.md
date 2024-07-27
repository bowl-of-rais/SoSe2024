```cpp
// compares registers with signed ints  
int Register::compare_int_registers(const Register& r1, const Register& r2) {  
    assert(r1.get_type() == Register::Type::INT64);  
    assert(r2.get_type() == Register::Type::INT64);  
  
    // check for signs  
    std::cout << "data r1: ";  
    for (int i = 0;i < 8; i++) {  
       std::cout << std::bitset<8>(r1.data[i]) << " ";  
    }  
    std::cout << "\ndata r2: ";  
    for (int i = 0;i < 8; i++) {  
       std::cout << std::bitset<8>(r2.data[i]) << " ";  
    }  
    std::cout << "\n";  
    size_t most_significant = (std::endian::native == std::endian::little) ? 7 : 0;  
    bool r1_pos = (r1.data[most_significant] ^ 0b10000000) == 0;  
    bool r2_pos = (r2.data[most_significant] ^ 0b10000000) == 0;  
  
    if (r1_pos != r2_pos) {  
       // differing signs: positive number is larger  
       return r1_pos ? 1 : -1;  
    } else {  
       // same sign: normal comparison if both positive  
       // if both negative: flipped comparison       int c = memcmp(&r1.data, &r2.data, sizeof(int64_t));  
       return r1_pos ? c : -c;  
    }  
  
}
```


```cpp
struct Comparison {  
   std::vector<Criterion>* criteria;  
   explicit Comparison(std::vector<Criterion>* c);  
   bool operator()(const std::vector<Register*>& t1, const std::vector<Register*>& t2) const {  
      for (auto c : *criteria) {  
         auto a1 = t1.at(c.attr_index);  
         auto a2 = t2.at(c.attr_index);  
         if (a1 < a2) {  
            return !c.desc;  
         } else if (a1 > a2) {  
            return c.desc;  
         }  
      }  
      return true;  
   }  
};

Sort::Comparison::Comparison(std::vector<Criterion>* c) {  
   this->criteria = c;  
}

```




# old hash aggregate

```cpp
if (next_group != groups.end()) {  
   // aggregate group   output = {};   aggregates[next_group->first] = {};   aggregates[next_group->first].reserve(aggr_funcs.size() + group_by_attrs.size());  
   for (auto f : aggr_funcs) {  
      switch (f.func) {         case AggrFunc::MIN:            aggregate_min(&f);            break;         case AggrFunc::MAX:            aggregate_max(&f);            break;         case AggrFunc::SUM:            aggregate_sum(&f);            break;         case AggrFunc::COUNT:            aggregate_count(&f);            break;      }   }  
   output.reserve(group_by_attrs.size() + aggr_funcs.size());   for (auto attr : group_by_attrs) {      output.emplace_back(&next_group->second[0][attr]);   }   for (size_t i = 0; i < aggr_funcs.size(); i++) {      output.emplace_back(&aggregates[next_group->first][i]);   }  
   next_group++;   return true;}  
return false;
```



```
void HashAggregation::aggregate_min(AggrFunc* f) {  
   Register aggr = next_group->second[0][f->attr_index];  
   for (auto t: next_group->second) {  
      if (t[f->attr_index] < aggr) {  
         aggr = t[f->attr_index];  
      }  
   }  
   if(aggr.get_type() == Register::Type::INT64) {  
      aggregates[next_group->first].emplace_back(Register::from_int(aggr.as_int()));  
   } else {  
      aggregates[next_group->first].emplace_back(Register::from_string(aggr.as_string()));  
   }  
}  
  
void HashAggregation::aggregate_max(AggrFunc* f) {  
   Register aggr = next_group->second[0].at(f->attr_index);  
   for (auto t: next_group->second) {  
      if (t[f->attr_index] > aggr) {  
         aggr = t[f->attr_index];  
      }  
   }  
   if(aggr.get_type() == Register::Type::INT64) {  
      aggregates[next_group->first].emplace_back(Register::from_int(aggr.as_int()));  
   } else {  
      aggregates[next_group->first].emplace_back(Register::from_string(aggr.as_string()));  
   }  
}  
  
void HashAggregation::aggregate_sum(AggrFunc* f) {  
   int64_t aggr = 0;  
   for (auto t : next_group->second) {  
      aggr += t[f->attr_index].as_int();  
   }  
   aggregates[next_group->first].emplace_back(Register::from_int(aggr));  
}  
  
void HashAggregation::aggregate_count(AggrFunc* f) {  
   int64_t aggr = 0;  
   for (auto t : next_group->second) {  
      aggr++;  
   }  
   aggregates[next_group->first].emplace_back(Register::from_int(aggr));  
}
```

```
if (!groups2.contains(key)) {  
    groups2[key] = {};}  
groups2[key].emplace_back();  
groups2[key].back().resize(aggr_funcs.size());  
for (size_t i = 0; i < t.size(); i++) {  
   groups2[key].back()[i] = *t[i];}
```