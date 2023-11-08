# LIRS Algorithm
- LRU replacement algorithm

## Reuse distance
- **recency:** distance from a block to the top of LRU stack
    - aka distance between last reference to current time
- **reuse distance:** distance between 2 consecutive references
    - deeper and more useful info
- **inter-reference recency (IRR):** number of other unique blocks accessed between 2 consecutive references to this block
    - needs another stack
- **low IRR (LIR) blocks**
- **high IRR (HIR) blocks**

## Low inter-reference recency set (LIRS)
- keep low IRR blocks in buffer cache
- replace high IRR blocks

### 2 stacks
- LIRS needs 2 stacks
- large LRU stack for low IRR blocks (LIR)
- small LRU stack for high IRR blocks (HIR)
- `cache_size = lir_cache_size + hir_cache_size`

### Measuring IRR
- after a hit to a high IRR block in the small stack, the block becomes low IRR and is moved to the large stack if it can also be found in the large stack
- otherwise, it goes to the top of the small stack.
- low IRR blocks at the bottom of the large stack will become high IRR blocks and go to the small stack when the large stack is full

## Low complexity of LIRS
- both recencies and IRRS are recorded in both stacks
- a block is low IRR if it's found in both stacks
- no need for comparisons or measurements
- LIRS = LRU = O(1)
    - needs stack pruning operations

## LIRS operations
- initialization
    - all referenced blocks are given a LIR status until LIR block set is full
- place resident HIR blocks in the small LRU stack when:
    - accessing an LIR block (hit)
    - accessing a resident HIR block (hit)
    - accessing a non-resident HIR block (miss)

## Benefits
- file scanning: 1-time access blocks will be replaced quickly (because of their high IRRs)
- loop-like accesses: a section of loop data is protected in low IRR stack
- accesses with distinct frequencies: frequently accessed blocks in short reuse distance won't be replaced

## Performance
- outperforms existing replacement algorithms in almost all cases

## Problems
- high implementation overhead
    - the operations are too expensive, OS must approximate
- high lock contention cost
- Clock-pro and BP-Wrapper address these issues

## Impact
- used as a benchmark for replacement algorithms
- pioneered using reuse-distance
- confirmed to outperform all other algorithms
- used in MySQL
