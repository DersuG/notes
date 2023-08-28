# 02
- **Moore's law:** # transistors will double every 2 years
    - Software has become inefficient
- **Dennard's law:** As # transistors go up, we can keep power consumption constant
- **ISA:** Instruction Set Architecture
- **Time unit:** Tasks are divided into sequences of standard execution time units
- **Multiprogramming model:** OS assigns time units to each task
- **Memory hierarchy:** Data blocks are moved up and down throughout it
- General purpose framework (one size fits all)
    - Non-inclusive to other systems
        - SIMD, data flow, domain specific, etc.
    - Flexible but inefficient
    - CPU and multicores (very flexible, low power/performance efficiency)
    - GPUs (low flexibility, efficient for SIMT parallelism)
    - FPGSa (very low flexibility, efficient)
    - ASICs (application specific integrated circuits) (no flexibility, very efficient)

## Hardware-software stack
- executable apps
- user domain
    - programming tools
    - compiler/assembler/debugger
- OS
- CPU
    - TLB
    - MMU
    - cache controller
    - memory controller
    - L1/L2/L3 cache
- Main memory
- Disk controller
- HDD/SSD
- OS

## What is assembly?
- ISA defines grammar and vocabulary

## Hardware-independent algorithms might be inefficient
- big-O notation is for # ops and assumes moving data is free
    - CPU cycles are cheap
    - data movement is the bottleneck of computing
- algorithms should be architecture-aware
    - because memory hierarchy
    - because parallelism
    - because hardware acceleration
- 3 pillars in algorithm design:
    - low computing complexity (comparison ops)
    - high data locality (low data movement)
    - high parallelism
- few hardware-independent algorithms are great in all 3 pillars

## First computers
- Atanasoff Berry Computer (ABC) (1937-1939)
- Z1-Z4 (1936-1945)
