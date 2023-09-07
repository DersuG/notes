# CSE 3421 Meeting 2023-08-31

## Homework

### Problem 1

#### Question 1
- **MIPS:** millions of instructions per second
- `MIPS = (num_instructions * 10^-6) / cpu_execution_time`

#### Question 2
- calculate percentage of each instruction class to calculate average effective CPI
- make sure the 20% increase is applied to overall CPI

## Abstractions
- each level of abstraction hide details of lower levels
- multilevel abstractions create top-down stack of more and more details
- the ISA is the layer between the upper logical levels and the lower hardware levels

## IBM compatibility problems in early 1960s
- by 1960s, IBM had 4 different lines of computers
    - each with their own ISA, I/O systems, software, etc.
- **IBM System/360:** standard ISA for all systems

## Unified ISA and standardized design of CPU
- **datapath:** registers and arithmetic/logic ops (ALU)
- **control unit:** operations based on instructions
- **instruction cache:** on-chip storage for frequently used instructions
- **data cache:** on-chip storage for frequently used data
- separate datapath (compute engine) and control unit

## RISC vs. CISC
- **CISC:** complex instruction set computers
    - hardware should support instructions that programmers want
        - optimize common instructions
        - complicated logic to support complex instructions
- **RISC:** reduced instruction set computers
    - only supports 1-cycle instructions
    - hardware should support only fast instructions (1 cycle)

## RISC architectures
- Alpha (DEC)
- ARC
- **ARM**
- AVR (Norway)
- **MIPS**
- PA-RISC (HP)
- PIC
- Power Architecture (PowerPC, IBM)
- SuperH (Hitachi)
- SPARC (Sun)
- AMD

## CISC architectures
- System/360 (IBM)
- PDP-11 (DEC)
- VAX (DEC)
- 68000 (Motorola)
- **x86 (Intel)**
- **AMD**

## MIPS-32 ISA
- instruction categories:
    - computational
    - load/store
    - jump/branch
    - floating point (via coprocessor)
    - memory management (PC)
    - special: HI and LO to store long words, high bits in HI, low bits in LO
- registers (32-bit):
    - R0 - R31
    - PC
    - HI
    - LO

### Instruction formats
- all 32-bits
- **R format:** `[op:6][rs:5][rt:5][rd:5][sa:5][funct:6]`
- **I format:** `[op:6][rs:5][rt:5][immediate:16]`
- **J format:** `[op:6][jump_target:26]`

### R-type instructions
- `add $t0, $s1, $s2`
- `sub $t0, $t1, $s2`
- each instruction has 3 operands

### Memory access instructions
- 2 basic instructions:
    - load word: `lw $t0, 4($s3)`
    - store word: `sw $t0, 8($s3)`
- the address format `4($s3)` means add 4 to the register to get the data address
