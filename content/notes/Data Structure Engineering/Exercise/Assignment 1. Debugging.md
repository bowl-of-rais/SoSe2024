- [x] do debugging task ✅ 2024-04-24

-----

topic: [[2.1 Hash tables]]

---
#### TL;DR

- use git
- `-O0 -g -Wall`
- use debugger
- `asserts`

#### Task: fix `buggyhashtable.cpp`

- asserts pass, no more sanitizer errors
- can also write more tests
#### Debugging

- actually use git, commit often
- distinguish: compiler warnings, compiler errors, linker errors, runtime errors
#### GDB things

- `gdb --args ./hashtable.out`
- `-Wall`: flag for compile-time warnings: always use
- `-ltbb`: *thread building blocks* library
- sanitizer: introduces runtime checks, gives info like mode, stack trace
- split view: code and terminal

useful commands: `next` or `n`, `print` or `p`, `p/t` to print binary

- `next` goes to next line, `step` goes into function

#### Undefined Behavior

- C++: compiler optimizes code when undefined behavior is encountered
- `-fsanitize=undefined` -> warnings
- should be fixed even if no bug (can be introduced by other optimization levels)

#### Memory Issues

- segfaults can be found by debuggers
- not always tho: `-fsanitize=address`

#### Assertions

- can be used to check assumptions (eg abt args)
- compiling with `-DNDEBUG` removes asserts. don't change anything in asserts.
- in code: `ensure` wraps asserts and throws errors in production

#### Program Localization

- use "binary search" for debugging
	- enable/disable things
	- eg single thread vs multi
	- also a reason to commit often: `git bisect`

#### Sanity Checks

take a step back. right file? right binary?
- add syntax error, compilation should fail
- or just print debug

#### Advanced Shit

- hardware watchpoints in gdb

---

# Debrief

##### Constructor

- use sizeof(Entry*)
- UD for size=0
- also nullptr may not be 0

##### Destructor

- memory leaks
- adapt size

##### Lookup

- mask missing

##### Insert

- move assignment behind initialization
- chaining

##### Erase

- fix broken links