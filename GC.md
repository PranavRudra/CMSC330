# Garbage Collection

## Lifecycle

- allocation: when memory is allocated to the program (allocator does this)
- lifetime: how long memory is used by the program
- recovery: when system recovers memory for reuse (allocator does this)

## Types

- static/fixed memory (fixed memory address)
- stack/LIFO memory
- dynamically allocated memory

## Memory Management

- primary goal: automatically reclaim dynamically allocated memory
- secondary goal: reduce fragmentation (refer to CMSC 216 for details)

## Reference Counting

- track how many pointers point to an object for each object
- if the reference count ever goes to 0, free that object
- pro
  - incremental technique (small amount of additional work for each additional allocation)
- con
  - cycles (i.e. cutting `A` from `A -> B -> C -> B` won't free `B` and `C` since they point to each other)
  - cascading decrement operations can be extremely computationally expensive and affect the runtime

## Tracing

- determine reachability as needed rather than maintain reference count
- every so often stop the world and free up dead/unreachable memory

### Mark-Sweep

- mark: trace the heap and mark all reachable objects
- sweep: go through heap and free all unreachable objects
- pro
  - no problem with cycles (`B`, `C` will be deallocated since they are unreachable)
  - live objects stay at the same memory location (there's no copying and moving)
- con
  - cost of the mark and sweep is proportional to heap size
  - fragmentation since available space still broken up

### Copying GC

- maintain two semispaces, only one of which is active at any given time
- at garbage collection time flip the semispaces and do:
  - trace live data starting at root set 
  - copy live data into other semispace
  - declare current semispace dead
  - switch into other semispace
- pro 
  - no fragmentation
  - only live data touched
- con
  - requires twice the memory
