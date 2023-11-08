# CLOCK-Pro
- approximation of LIRS based on CLOCK infrastructure
- pages are in 2 groups based on reuse distance (or IRR):
    - cold pages
    - hot pages
- 3 hands:
    - hand-hot for hot pages
        - finds hot pages to demote into cold pages
    - hand-cold for cold pages
        - finds pages for replacement
    - hand-test for running a reuse distance test for a block
        - checks if a cold page should be promoted to hot
        - evicts non-resident cold pages
- allocation of pages between hot pages (`Mhot`) and cold pages (`Mcold`) are adaptively adjusted (`M = Mhot + Mcold`)
- hot pages:
    - all hot pages are resident (LIR blocks)
- cold pages:
    - some cold pages are also resident (HIR blocks)
        - either a fresh replacement
        - or demoted from a hot page
    - keep track of recently replaced pages (non-resident HIR blocks)

## Concurrency in buffer management
- concurrent accesses to buffer caches need critical sections
- buffer cache (pool) keeps hot pages
- hit ratio depends a lot on replacement algorithm

## Accurate algorithms and their approximations
- LRU -> CLOCK
- LIRS -> CLOCK-Pro
- ARC -> CAR
- clock sets bit to 1 without lock for page hit
- lock synchronization only used for misses
- clock approximation reduces lock contention but reduces hit ratios

## Hit ratio vs. lock contention
- high hit ratio:
    - LRU-k
    - 2Q
    - LIRS
    - ARC
    - SEQ
- high scalability (low lock contention):
    - CLOCK
    - CLOCK-Pro
    - CAR
- approximating algorithms with clocks is hard
    - not always possible

## Ways to reduce lock contention
- batch requests
- prefetching

## Impact
- adopted into FreeBSD/NetBSD
- patches available for linux

## Performance of multicore architecture
- cache contention and pollution
    - Conflict cache misses among multi‐threads can significantly degrade performance.
- memory bus congestion
    - bandwidth is still limited
- "disk wall"
    - disk is still slow
- doesn't actually scale super well

## OS cache partitioning in multi-cores
- executes LIRS principle in LLC in multicores
- implementations in linux:
    - static partitioning
        - Predetermines the amount of cache blocks allocated to each running process at the beginning of its execution
    - dynamic cache partitioning
- status:
    - open source in linux kernels
    - adopted as a software solution in Intel SSG in May 2010
