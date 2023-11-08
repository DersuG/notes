# Relocation

## Absolute programs
- programmer decides where everything will be in memory

## Load time flexibility
- os decides where to things will be in memory

## Handling relocation
- loader updates some parts of some text records
    - must know load address
- assembler must:
    - create object file as if load address was 0
    - tell loader which parts need to be updated
- convention: omit operand of `ORIG` to indicate a relocatable program

## Alternatives
- this approach could double the size of an object file
- bit-masking:
    - compact representation of which bytes need to be updated
    - difficult to read/grade/debug
- in-line modification flags: (this is what we'll do)
    - text records have a suffix, marking them as absolute, relocatable-9, and relocatable-16

## Problems with page boundaries
- we're only gonna relocate to page boundaries

## Symbol table with relocation
- exact design is up to us
- e.g. 3 columns: `symbol_name`, `value`, and `is_relative`
    - don't yet care if they are relocatable-9 or relocatable-16 (this first pass is just processing the symbols, and one symbol could be used in 2 different ways)
- implicitly defined symbols are relative, explicitly defined symbols (`.EQU`) may not be

## Relative literals
- literals whose values are relative
