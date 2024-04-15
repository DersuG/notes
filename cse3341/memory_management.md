# Memory Management

## Live and dead objects
- **live object:** will be used in the future
- **dead object:** won't be used anymore
- deallocate asap after last use, but not before

## Explicit/manual memory management
- more code
- global reasoning
- correctness is hard
- can be very efficient

## Automatic memory management
- much safer/easier
- not perfect
- worse performance, but it's getting better

## Garbage
- any object that will never be referenced again
- in practice, anything that can't be reached
- managed languages couple GC with **safe pointers**
    - no arbitrary memory access
    - compiler can identify and provide all pointers to the GC
        - enforces "once garbage, always garbage"
    - runtime system can move objects and update pointers
- reachability over-approximates liveness
    - live objects are a subset of reachable objects

## Garbage collection
- two types:
    - reference counting
        - works proportional to dead objects
        - memory freed immediately
        - cycles are problematic (2 objects pointing to each other)
    - tracing
        - works proportional to live objects
        - memory isn't freed immediately
        - can be concurrent

## Tracing GC
- **roots:**
    - local variables (registers, stack locations)
    - static variables
    - "temporary" roots (see quandary documentation)
- starting at roots, recursively visit all accessible objects
    - determining reachability
- when does it happen?
    - **stop-the-world:** safe points inserted by VM
    - concurrent
    - incremental
- how many threads?

## Heap organization
- allocation
    - free list vs bump allocation
- identification
    - tracing (implicit) vs. reference counting
- reclamation
    - sweep-to-free vs. compaction vs. evacuation

![[Pasted image 20240325131255.png]]
## Mark-and-sweep implementation
- free-lists sorted by size
    - blocks of same size, or
    - individual objects of same size
- most objects are small (< 128 bytes)
- allocation
    - mark phase
        - compute reachability on the heap, mark all live objects
    - sweep phase
        - sweep memory for free objects, and populate the free-lists

## Semispace
- fast **bump pointer** allocation
- requires copying collection
- reserve half the heap to copy to, in case all objects are live
- not incremental, must free en masse
- mark phase:
    - copies object to the to-space when collector first encounters it
    - installs **forwarding pointers**
    - updates pointers as it goes
        - **transitive closure**
        - basically, copies all objects the objects points to
- pros:
    - fast allocation
    - locality of objects allocated around the same time
    - locality of objects that reference each other
- cons:
    - wastes space

## Generational GC
- **generational hypothesis:** young objects die quicker than old ones
    - most pointers are from younger objects to older objects
- sort heap by object age, prefer to collect young objects
- divide heap into young-space and old-space
- when young-space fills up:
    - collect it and copy objects to old-space
- when old-space fills up:
    - collect both spaces
    - generalizing to m generations: if space n < m fills up, collect all spaces i <= n
- **unidirectional barrier:**
    - record only old-to-young pointers
    - don't need to record young-to-old pointers, since we never collect old-space by itself
    - track the **barrier** between young objects and old spaces

## Weaknesses of reference counting
- detecting loops in object references

## Weaknesses of tracing
- 