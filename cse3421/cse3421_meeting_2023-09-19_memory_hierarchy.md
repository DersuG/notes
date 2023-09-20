# CSE 3421: Meeting 2023-09-19

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
