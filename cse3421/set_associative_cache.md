# Set Associative Cache
- instead of cache index there is set index
- **set index:** selects which set

## Range
- each additional bit of the set doubles the # blocks/set
- making tag bigger / set index smaller increases associativity

## Costs
- when a miss happens, which way's block do we pick for replacement
    - **least recently used (LRU):** replace the block that has been unused the longest
        - needs hardware
        - need a reference bit for each block (0 = not-accessed, 1 = accessed)
- cost of N-way set associative cache
    - N comparators (delay and area)
    - MUX delay (set selection)
    - data is only available after set selection and hit/miss decision, whereas in a direct-mapped cache, the data is available before hit/miss decision

