# Pipelining
- start fetching and executing the next instruction before the current one is done
- ideally, the speedup is approximately equal to the # of pipeline stages (e.g. a 5 stages gives a x5 speedup)
- assumes the next instruction has no dependency with the current one (e.g. not a conditional jump)
- improves **throughput** (total amount of work done in a time unit)
- doesn't improve **latency** (how long a single instruction takes)
    - latency is often worse because we are limited by the slowest stage
    - single-cycle design can have stages with different speeds
- clock cycle is limited by the slowest stage

## 5 stages of execution
- **IF:** fetch instruction + update PC
- **DEC:** decode instruction + fetch registers
- **EX:** execute r-type or calculate memory address
- **DM:** read/write from memory
- **WB:** write back to register file

## Pipelining performance
- `cpu_time = latency + cycle_time * (num_instructions - 1)`
- `pipelined_execution_time = num_instruction / num_stages + overhead`
- once pipeline is full, `CPI = 1`

## Pipelining MIPS
- easy because:
    - all instructions are the same length
    - memory operations only happen in loads and stores
    - instructions write at most one result, and does it in the last stages
- after each stage is a buffer, which is the input of the next stage
    - `[IFetch] -buffer-> [Dec] -buffer-> [Execute] -buffer-> [MemAccess] -buffer-> [WriteBack]`
    - control signals are also buffered
- shortcuts:
    - skip ALU
    - skip memory access

## Pipeline hazards
- **structural hazard:** 2 instructions attempting to use the same resource at once
    - e.g. only one data memory, one instruction writes while another instruction is being fetched
    - reading/writing from register file only takes half a cycle, so we can make all writes happen in the first half and reads in the second half, fixing a structural hazard
- **data hazard:** attempting to use data before it's ready
    - e.g. one instruction writes to register, next instruction needs to read from register
    - can fix by stalling, but this hurts CPI
    - can also fix by reading from the buffer after the first instruction's ALU as the input of the second instruction's ALU (forwarding)
- **control hazard:** attempting to make a decision about control flow before condition has been evaluated and PC updated
- can usually be resolved by waiting
- need to detect

## Data forwarding (aka bypassing)
- **forwarding:** send results as soon as they're available to wherever they're needed
- take result from earliest point that exists in any pipeline state registers and forward it to the functional units (e.g. ALU) that need it that cycle
- ALU inputs can come from any pipeline register, not just ID/EX by:
    - adding multiplexors to ALU inputs
    - connecting Rd write data in EX/MEM or MEM/WB to either (or both) of the EX's stage Rs and Rt ALU mux inputs
    - controlling the new multiplexors properly
- other functional units may need similar forwarding logic
- can achieve `CPI = 1` even with dependencies

## Control hazards
- less frequent than data hazards, but no truly effective solutions
- caused by:
    - unconditional branches
    - conditional branches
    - exceptions
- possible approaches:
    - stall
    - move decision point as early in the pipeline as possible, reducing stall
    - delay decision (requires compiler support)
    - predict

## Branch prediction
- always read the next instruction (predict that branch will fail)
- if we're wrong, we need to flush the pipeline (losing ~3 instructions)
- we can have special hardware (an ALU for comparisons) to make branch decision as early as possible
    - now we only lose 1 instruction!
- longer pipelines mean more instructions have to be flushed
