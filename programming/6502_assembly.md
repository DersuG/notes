# Intel 6502 Assembly

## Resources
- [easy 6502](http://skilldrick.github.io/easy6502/)
- [6502.org tutorials](http://www.6502.org/tutorials/)

## Syntax
- `#` precedes literals, anything else is a memory address
- `$` precedes hex
- `#$0f` is a hex literal
- `$0f` is a hex memory address
- `;` is a comment
- `<label>:` is a label
```6502
LDA #$c0 ;load $c0 into register A
TAX      ;transfer (copy) register A to register X
INX      ;increment register X
ADC #$c4 ;add $c4 to register A
BRK      ;break
```

## Registers
Registers hold 1 byte.
- `A` - accumulator
- `X`
- `Y`
- `SP` - stack pointer
- `PC` - program counter

## Flags
- All flags are stored as a bitfield in 1 byte.
- They work like x86 flags (are set depending on what happened during the last instruction)
`NV-BDIZC`
- `C` - carry flag
- `Z` - zero flag

## Instructions
- Load: `LDA|LDX|LDY <literal|address>`
- Transfer: `TAX|TXA|TAY|TYA`
- Store: `STA|STX|STY <address>`
- Increment: `INX|INY`
- Add (with carry): `ADC <literal|address>`
- Subtract (with carry): `SBC <literal|address>`
- Break: `BRK`
- Branch if not equal: `BNE <label>`
    - Branches if `Z` flag is 1.
- Branch if equal: `BNE <label>`
    - Branches if `Z` flag is 0.
- Branch on carry clear: `BCC <label>`
    - Branches if `C` flag is 0.
- Branch on carry set: `BCS <label>`
    - Branches if `C` flag is 1.
- Compare `CPX|CPY <literal|address>` (?)
    - Sets `Z` flag to 1 if equal, 0 otherwise.
- Push accumulator: `PHA`
    - Pushes from `A` to stack
- Pull accumulator: `PLA`
    - Pops from stack to `A`
- Jump: `JMP <address>`
    - Unconditional jump
- Jump to subroutine: `JSR <label>`
- Return from subroutine: `RTS <label>`

## Branching
- Branching can only shift execution up to 256 bytes forward or back.
- To move further you need jumps.

## Addressing modes
- 16-bit address bus means 65536 bytes of memory, represented as `$0000` through `$ffff`
- absolute:
    - full memory location used
    - ie: `STA $C000`
- zero page: `$c0`
    - all instructions that support absolute addressing support zero page addressing
    - address is 1 byte
    - only first page of memory is available (256 bytes)
    - faster, smaller
- zero page X: `$c0,x`
    - same as zero page addressing, but `X` register is added
    - overflows wrap around
    - supported by `LDA`, `LDY`, `STA`, `STY`
- zero page Y: `$c0,y`
    - same as zero page X, but for `Y` register
    - supported by `LDX`, `STX`
- absolute X and absolute Y: `$c000,x` and `$c000,y`
    - like zero page X and zero page Y, but for absolute addressing
    - absolute X supported by `LDA`, `STA`
    - absolute Y supported by `LDA`, `STA`
- immediate: `#$c0`
    - doesn't deal with addresses
    - instructions like `LDX #$ff` which have a literal argument
- relative: `$c0` or label
    - used in branching instructions
    - 1 byte offset address, jumps up to 256 bytes forward/backwards
- implicit:
    - instructions like `INX` which don't take a memory location argument
- indirect: `($c000)`
    - uses an absolute address to look up a 2-byte address
    - like `[]` in x86
    - supported by `JMP`
- indexed indirect: `($c0,x)`
    - add `X` register to zero page address, then look up 2-byte address
- indirect indexed: `($c0),y`
    - uses zero page address to look up a 2-byte address, then adds `Y` register

## Architecture
- little-endian

## Stack
- $0100 to $01ff
- pushes / "pulls" 1 byte at a time
- stack pointer starts at $ff (corresponds to $01ff)
  - pushing a byte moves stack pointer to $fe ($01fe)

## Jumps
- `JMP` jumps to a 2 byte absolute address
- `JSR` pushes the address of the next instruction minus 1 to the stack
- `RTS` pops the address and adds 1

## Assembler constants
- `define <name> <value>`
- can use letters, digits, underscores
- constants are defined like `define a_dozen $0c` and used like `LDX #a_dozen`


