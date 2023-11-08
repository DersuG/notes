# Project 2: Assembler

## Pass 1
- builds symbol table
    - n column: name
    - v column: value
    - r/a column: is value absolute or relative?
- builds literal table
    - n column: name
    - v column: value
    - addr column: address
- eats comments
- if `.ORIG` has no operand, start assembling at 0 (`LC = 0`)
- outputs intermediate format for pass 2

### Building table
1. read line
2. if there's a symbol name (e.g. `msg .STRZ "hi!"`):
    1. get (pseudo) operator
    2. get value
        1. all symbols, except for `.EQ`, get their values from the location counter `LC`
        2. `.EQ` gets its value from its operand
3. if there's a literal (e.g. `    LD R6,=#100`):
    1. get name
    2. get value
    3. figure out the address at the end of pass 1
4. increment `LC` by size of (pseudo) operator

## Pass 2
- builds header and text records
- only relative symbols from symbol table get us a modifiable record
