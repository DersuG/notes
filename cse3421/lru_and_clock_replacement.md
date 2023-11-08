# LRU and Clock Replacement Algorithms

## Replacement algorithm
- decides what data to evict when full
- used everywhere
- simple concept, hard to optimize

## Software/hardware support
- dynamically record the access status of each block
- uses global and local info
    - local: a hardware reference bit for each block (fast but low accuracy)
    - global: all blocks are ranked with software stack (relative slow but more accurate)

## LRU replacement
- most common replacement strategy
- blocks are sorted in LRU order
    - blocks enter at the top (MRU)
    - blocks leave from the bottom (LRU)
- **recency:** distance from a block to the top
- on hit: move block to top

## LRU problems
- can't deal with certain access patterns
    - file scanning
        - one-time data evicts to-be-reused data
        - common data access pattern
    - loops
        - a loop size of `k+1` will miss `k` times for an LRU stack of size `k`
    - different access frequencies
        - for B-trees, the index structure is accessed much more frequently than records. Infrequently used records can evict frequently used indices, degrading data access performance
- The assumption of “recently used will be reused” is not always right

```c
/* if LRU stack buffer is size 4, all accesses to A will be misses */
for (int i = 1; i <= n; i++){
    for (int j = 1; j <= 5; j++){
        A[j] = A[j] + 1;
    }
}
```

## Fixing LRU
- LRU and LIRS are too expensive, OS can only approximate
- 2 types of efforts:
    - detect specific access patterns
    - learn insights into accesses with complex algorithms
    - (most published methods are impractical)
- solutions:
    - The LIRS algorithm (SIGMETRICS'02)
    - Clock-pro: a system implementation (USENIX'05)
    - BP-Wrapper: lock-contention free assurance (ICDE'09)
- problems with LIRS (addressed by Clock-pro and BP-Wrapper):
    - high overhead when implemented
        - replacement algorithms define a set of operations that are used during data access, but it's too expensive
        - in practice, you have to approximate with reduced operations
    - high lock contention cost
        - **lock contention:** stacks must be locked for concurrency
        - lock contention limits the scaleability

## The clock algorithm
- LRU approximation
- blocks are cached in a circular list
- each block has a reference bit (1=accessed, 0=not accessed)
- a hit sets the reference bit of that block
- on a miss, the clock hand sweeps from its current position until it finds a block with an unset reference bit
    - (so find the next block with an unset reference bit)
    - that block is evicted
    - all the blocks that were passed on the way have their reference bits unset
- it will always be able to evict a block (passed blocks will have their reference bits unset)
- many hits = clock hand moves slowly
- many misses = clock hand moves quickly
- doesn't necessarily evict LRU blocks, but does evict blocks that weren't recently used
