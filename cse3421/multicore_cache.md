# Cache and Multicores
- each core gets its own L1 and L2 cache
- L3 cache is shared

## Performance issues
- **cache pollution:** one-time data flushes out more useful data
- cache contention and pollution
    - conflict misses across cores
- memory bus congestion
    - bandwidth is limited as # of cores increases
- "disk wall"
    - multicores also demand high disk throughput

## Last level cache
- **last level cache (LLC):** lowest level of cache, shared across cores (e.g. L3)
- **LLC conflicts:** total working set is too big
- contention in memory bus makes things even slower

## OS cache partitioning in multicores
- physically indexed caches are divided into multiple page regions
    - **page color:** the page region
- during address translation (e.g. in the page table), OS can directly allocate pages in the LLC to each process by selecting a physical page with the specific color bits

![[cache_color_bits.svg]]

- OS can partition the cache
- shared cache is partitioned between programs via OS address mapping
- *p* is the bits used by the OS to map pages into cache
- OS manipulates cache by manipulating the address mapping

## OS-based cache partitioning
- static cache partitioning
    - predetermine amount of cache blocks allocated to each process
    - divides shared cache to multiple regions and partitions cache regions via OS page address mapping
- dynamic cache partitioning
    - adjusts cache quota among processes dynamically
    - page re-coloring
    - dynamically changes processes' cache usage through OS page address remapping
- both implemented in Linux and adopted by Intel SSG

## Cache allocation technology (CAT)
- by Intel in 2016
- another approach to cache partitioning
- LLC has high associativity with many ways
- OS partitions cache horizontally
- bitmask is used to partition the cache vertically
    - each process manages its own bitmask
    - bit1=1 means cache way 1 is accessed by the process
    - bitmask represents a **class of service (CLOS)** for each process
    - some ways are reserved for the system (OS?)
- the default setting means LLC is shared by all processes
![[Pasted image 20231107101453.png]]
- `CLOS[0]` exclusively owns 4 ways of cache
- `CLOS[1]` exclusively owns 4 ways of cache
- `CLOS[2]` and `CLOS[3]` share 6 ways of cache
- `CLOS[3]` exclusively owns 6 ways of cache
