# CSE 3421: Meeting 2023-09-12

## Homework 2: Problem 5
- answer for last instruction `bne R7, R5, Loop` 
    - `5 7, 5, -4`
    - `000101 00111 00101 1111111111111100`
    - the jump offset is -4 instead of -3 because  PC will be incremented by 4 bytes
    - the jump offset is in words because it is bitshifted internally to convert it to bytes

## Datapath and control
- simplified MIPS implementation:
    - memory-reference instructions: `lw` and `sw`
    - arithmetic-logical instructions: `add`, `sub`, `and`, `or`, and `slt`
    - control flow instructions: `beq` and `j`

## Clocking methodologies
- **clocking methodology:** when data in a state element is valid and stable
- typically: read contents of state element -> send values through combinational logic -> write results to state elements

## Fetching instructions
- the PC is used to address into instruction memory
- the PC is incremented by 4

## Decoding instructions
- send fetched instruction's opcode and field bits to control unit

## Executing R-format instructions
- for `add`, `sub`, `slt`, `and`, and `or`
- format: `[op:6][rs:5][rt:5][rd:5][shamt:5][funct:6]`
- use instruction addresses to fetch data
- there's a register write signal, registers only change when this signal allows it

## Executing load and store operations
- get the base register
- sign-extend the offset field in the instruction
- add the base register to the sign-extended offset

## Executing branch operations
- compare operands
- calculate branch target address by adding the (incremented) PC to the sign-extended offset field
- set the PC to the target address

## Executing jump operations
- ???? see slides

## Multiplexors
- switches between multiple input signals using binary control lines

## Creating a single datapath from the parts
- assemble datapath segments and add control lines and multiplexors as needed
- **single cycle design:** fetch, decode, and execute each instruction in 1 clock cycle
  - no datapath resource can be used more than once per instruction, so some must be duplicated (e.g. separate instruction memory, data memory, adders)
  - multiplexors needed at input of shared elements
  - write signals to control writing to the register file and data memory
- cycle time is determined by the length of the longest instruction path
  - so the CPU just waits after a fast instruction completes

## Decoding R-type instructions
- multiplexor control lines are set
  - `Branch = 0` (don't add branch offset to PC)
  - `MemToReg = 0` (don't store memory data to registers)
  - `ALUSrc = 0` (source 2nd ALU operand from register, not an immediate)
  - `RegWrite = 1` (allow registers to be written to)
  - `RegDst = 1` (set destination register)

## Decoding `lw` instruction
- multiplexor control signals are set:
  - `Branch = 0` (add branch offset to PC)
  - `MemToReg = 1` (write memory data to registers)
  - `ALUSrc = 1` (source 2nd ALU operation from immediate)
  - `RegDst = 0` (set destination register)

What if the actual bits in the instruction directly controlled the multiplexors?

