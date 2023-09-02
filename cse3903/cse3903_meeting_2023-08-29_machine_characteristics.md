# Machine Characteristics

## Capabilities
- read/write from paged memory
- write strings and decimal numbers to console
- read single characters and decimal numbers from console
- read a program as a text file ("loader")

## Memory
- 16-bit word-machine
- 65536 words
- 128 pages of 512 words

## Registers
- **program counter:** 16 bit address of next instruction (not current instruction)
- **general purpose registers:** 8 16-bit registers (R0, R1, ..., R7)
- **condition code registers:** 3 1-bit registers (N, Z, P)
    - change every time a value/result is written to a general purpose register (except when writing return addresses into R7)
    - N is set iff value was negative
    - Z is set iff value was zero
    - P is set iff value was positive
    - don't change if no write happened

## Arithmetic
- fixed-point only

## Data processing instructions
- ADD
- AND
- NOT
- 5-bit operands are sign-extended to word size

## Data movement instructions
- LD
- LDR
- LDI
- LEA
- ST
- STR
- STI

## Control flow instructions
- BRx
- JSR
- JSRR
- RET
- TRAP
    - OUT
    - PUTS
    - IN
    - HALT
    - OUTN
    - INN
    - RND
