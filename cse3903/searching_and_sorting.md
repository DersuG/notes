## Robustness
- what if key is not in table?
    - return arbitrary value (e.g. initial value)
    - return a special value (e.g. null, error code, etc.)
    - throw exception

## Open-address hashing
- flavors: where to look for open slot?
    - linear
    - quadratic (double the hop distance)
    - rehash with another function
- rehashing tradeoffs
    - eliminates clustering
    - computationally expensive
    - requires a family of independent hash functions
