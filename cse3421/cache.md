# Cache

## Cache terminology
- **cache capacity / cache size:** number of bytes of cache data, excluding tags, valid bit, etc.
- **cache block size:** number of bytes for each cache block, which determines the number of bits for the byte offset
- **number of bits for cache line:** number of bits for cache data block + number of bits for tag + valid bit + any other stuff
- **total number of bits for cache:** number of cache blocks * number of bits for a cache line

## Write allocate vs. no-write allocate
- **write miss:** data to be written to is not in cache
    - can just forward the write request to memory
- **write-allocate cache:** cache that allocates cache lines on a write miss
    - good for temporal locality
- **no-write allocate cache:** cache that doesn't allocate lines on a write miss
    - reduces complexity, temporal locality isn't always right
- write hits
    - require cache and memory to be consistent:
        - always write data to both cache block and next level of memory hierarchy (slow!)
        - can use a write buffer to make it faster
    - allow cache and memory to be inconsistent:
        - write data only to cache (it will be written to next level of memory hierarchy when the block is evicted)
        - need a **dirty bit** for each block to tell if it needs to be written back to memory when evicted
        - can use a write buffer for write-backs of dirty blocks
- write-back vs. write-allocate:
    - write-back means writing to memory when writing to a cache entry with a set dirty bit
    - write-allocate means moving memory to cache when it gets written to

## Measuring cache performance
- `cpu_time = num_instructions * cpi * cycle_time`
- `cpu_time = num_instructions * (ideal_cpi + memory_stall_cycles) * cycle_time`
- `cpi_ideal` is CPI without memory accesses (a cache hit)
- `avg_cpi = cpi_cache_hit + cache_miss_rate * cpi_cache_miss`
    - `cpi_cache_miss` = hit time in next level of cache
- e.g. a processor with ideal CPI of 2, a 100 cycle miss penalty, 36% of instructions are access memory, and has 2% `I$` and 4% `D$` miss rates:
    - `memory_stall_cycles = 2% * 100 + 36% * 4% * 100 = 3.44`
    - so `cpi_stalls = 2 + 3.44 = 5.44`
    - so 63% of time is spent in memory stall
- as CPI decreases or clock speed increases, the % of time spent in memory stall for the same cache increases

## Reducing miss rates
- allow more flexible block placement:
    - increase associativity (how many blocks an address can be mapped to)

## Cache misses
- **compulsory miss:**
    - cold start, process migration, first start, cold misses
    - first access to a block
    - solution: increase block size
        - increases miss penalty
        - very large blocks could increase miss rate
- **capacity miss:**
    - cache can't contain all blocks used by program
    - solution: increase cache size (may increase access time)
- **conflict miss:**
    - multiple memory locations mapped to same cache location
    - solution 1: increase cache size
    - solution 2: increase associativity (increases access latency)
    - **ping-pong effect:** constant conflict misses due to repeatedly accessing 2 addresses that map to the same block

## Reducing miss rates via multi-level caches
- **I$:** instruction cache
- **D$:** data cache
- **UL2$:** unified L2 cache
- for Intel Nehalem and AMD Barcelona:
    - L1 cache is split I$ and D$
    - L2 cache is unified
    - L3 cache is unified
    - all 3 are write-back and write-allocate

## Types
- [[direct_mapped_cache]]
- [[set_associative_cache]]

`avg_access_latency = hit_latency + miss_rate * (mem_latency + miss_penalty)`
