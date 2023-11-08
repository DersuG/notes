# Multi-column Cache

## 3 steps for cache miss
1. sending a memory address (address bus)
2. loading data from memory
3. transferring data to cpu (data bus)

## Hit ratio vs. latency
- set-associative caches get ~30% higher hit ratios than direct-mapped caches, but have high latency
    - also high power consumption

## Way prediction
- first hit
    - `cost = way_prediction`
    - same as direct-mapped cache
- non first hit
    - `cost = way_prediction + selection_in_set`
- worst case: miss
    - `cost = way_prediction + selection_in_set + miss`
- potentially lower latency

## Multi-column caches
- goals:
    - uses way prediction
    - maximize # of first hits
    - minimize latency of non-first hits
    - extra hardware must be simple and cheap
- address format: `[tag [major_location]] [set] [offset]`
    - **major location:** determine which way to check first

## Selected locations
- **MRU (most recent used) block:** most recently used block, either loaded from memory or just accessed
    - contained in the way pointed to by the major location bits
- **selected locations:** locations in the set other than the major location, where non-MRU blocks will be stored
    - if other locations are used for their own major locations, there won't be space for selected ones
- **swap:** process of swapping a block in/out of a major location
    - a block in a selected location gets swapped to the major location as it becomes MRU
    - a block in the major location is swapped to a selected location after a new block is loaded into it from memory
- location index (idk actual name)
    - tracks which location is the major location
    - tracks which locations are the selected locations
    - each way has bits, where a 1 means the location corresponding to that bit's position is a selected location

## Performance of multi-column caches
- hit ratio to major locations is ~90%
- hit ratio is higher than direct-mapped caches
- first hit cost is equivalent to direct-mapped
- non-first hit cost is still faster than set-associative caches

## MRU cache
- **MRU bitmap:** remembers the most recently used way
    - used for the way prediction in multi-column caches
    - 1 bit needed for a single way, 2 bits needed for 4 ways, etc.
    - after checking predicted way, the bitmap is checked to see if the data is in a selected location

is there not a way to check all tags in a set in parallel?
