
- most importantly: avoid cache invalidation
- one way to do it: pad data/use `alignas` keyword to make entries the size of a cache line 
	- assignment template has function for this: `cacheline.hpp`

----

##### Problems in `parallel_for` from previous exercise

- stragglers
	> Because all threads  must be synchronized, all threads will proceed at the speed of the slowest thread in each iteration
- every call spawns and destroys threads
- only works for task_size > threads

### Part 1 of assignment: improvements
##### Better version:

```cpp
struct Scheduler {
	std::vector<std::thread> workers;
	
	std::atomic<uint64_t> a;

	explicit Scheduler(unsigned threads) {
		for (auto tid = 0u; tid < threads; ++tid) {
			workers.emplace_back([&, tid]){
			
			}
		}
	}
	
	~Scheduler() {
		for (auto& t : workers) {t.join();}
	}

}

```

##### About stragglers

- atomic counter that identifies next item of work?
	- big scheduling overhead
- portion work into morsels (calculate size)

### Part 2 of assignment: TBB Counter

- sum up in local counter within loop and then add to global? -> expensive, needs to be done for every morsel

```cpp
#include <tbb/enumberable_thread_specific.h>

// initialize 
tbb::enumerable_thread_specific<std::unordered_map<uint64_t, std::string>> localStorage(
return std::unordered_map<uint64_t> std::string(initialSize);
});

// access
auto& myHT = localStorage.local()
```

how to synchronize a hashtable
- mutexes for each bucket (chaining)
- chaining hashtable from first assignment, make pointers atomic, CAS
	- also pad entries
- open hashtable: 1 atomic per item, with padded entries (cache-line size) to avoid false chaining (hashtables after each other in memory)
	- function for padding is provided

### Debugging tips

- try with single thread first
- lock from coarse to fine granularity
- what could other threads do between lines
- when in doubt, add a lock

-----

`local()`
- check hashtable for entry, create new one if id not found (`std::thread::id`)
- not called too often
- padding: pack thing? static assert, 

`parallelFor`
- store function? so that workers can access
- figure out morsel size
- store TASK, notify threads, wait for lock with function that sets state to idle
	- condition variable or use wait/notify from atomics
- wait for everyone to finish