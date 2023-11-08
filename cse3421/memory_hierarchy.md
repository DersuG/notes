# Memory Hierarchy

## Homework hints

### Problem 1
- straightforward, look at lecture slides to see how `lw` works
- reading from memory outputs 4 bytes, need to select the right byte with MUX `ALU[0-1]`
- the byte is then zero-extended
- MUX control signal `bu` needs to be 1 to select the byte, otherwise the whole word will be used when writing to register file

### Problem 2
- use the diagrams from the slides
    - `[IM] [Reg] [ALU] [DM] [Reg]`
    - ALU and DM have shortcuts

## Performance gains/losses
- experiments show branch prediction improves performance
    - a misprediction costs 2-3 cycles
- all modern CPUs use branch prediction
    - many dynamic methods
- pipeline structure makes a big impact

## Other pipeline structures
- flexible stage length:
    - multiplication is slow, let it take 2 cycles
    - mul component skips ALU and DM
- ARM7:
    - `[IM] [Reg] [EX]`
    - ALU, DM access, shift/rotate, and write-back are all at the end
- Xscale (Intel):
    - `[IM1] [IM2] [Reg] [SHFT] [ALU] [DM1] [Reg+DM2]`

## Von Neumann model
- memory contains data and instructions
- computing unit for arithmetic and logic ops
- control unit to interpret and execute instructions
- `[computing_unit] <-[communication_channel]-> [memory_unit]`

## SEAC
- Standards Eastern Automatic Computer
- 1950
- first stored-program machine
- IO mechanisms for data processing
- time-sharing
- first operational architecture based on Von Neumann model

## Memory structure
- cache
    - whenever addressing data in main memory, first check same address in cache
- main memory
- secondary memory (disk)
- large, fast, cheap, pick any 2
    - with hierarchy and parallelism we can give the illusion of having all 3
- typical hierarchy (speed in approximate cycles):
    - register file
    - ITLB + DTLB
        - L1
        - 1 MB
        - 1 cycle
    - instruction cache + data cache
        - L2
        - 10 MB
        - 10 cycle
    - second level cache (SRAM)
        - L3
        - 100 MB
        - 10 cycles
    - main memory (DRAM)
        - 100 cycles
        - gigabytes of data
    - secondary memory (disk)
        - 10000 cycles
        - terabytes of data

## Data movement costs
- **flop:** # of floating point operations per second
    - useful for comparing arithmetic capability of supercomputers
- pretty much anything not in the cpu will take >1 flop to access

## Memory hierarchy terminology
- **temporal locality:** if a memory location is accessed, it tends to be accessed again soon
- **spatial locality:** if a memory location is accessed, the locations around it tend to get accessed soon
- **block (aka cache line):** the basic data unit that is present in a cache
- **hit rate:** the fraction of memory accesses found in a level of the memory hierarchy
    - **hit time:** time to access that level
        - `hit_time = access_time + hit_detection_time`
- **miss rate:** the fraction of memory accesses not found in a level of the memory hierarchy
    - `miss_rate = 1 - hit_rate`
    - **miss penalty:** time to replace a block in that level with the corresponding block from a lower level
        - `miss_penalty = access_time + transmit_time + insert_time`
- `hit_time << miss_penalty`

## Data sizes

| unit | abbreviation | bytes |
|---|---|---|
| kilobyte | KB | 2^10 ~ 10^3 |
| megabyte | MB | 2^20 ~ 10^6 |
| gigabyte | GB | 2^30 ~ 10^9 |
| terabyte | TB | 2^40 ~ 10^12 |
| petabyte | PB | 2^50 ~ 10^15 |
| exabyte | EB | 2^60 ~ 10^18 |
| zettabyte | ZB | 2^70 ~ 10^21 |
| yottabyte | YB | 2^80 ~ 10^24 |
