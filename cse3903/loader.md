# Linking Loader

## .EXT
- `.EXT <symbol>`
- declares an external symbol
- error if the symbol is locally defined

## .ENT
- `.ENT <symbol`
- declares a symbol to be global
- "entry point"
- error if symbol is not locally defined

## BSS loader
- binary symbolic subroutine (BSS)
- one of the earliest relocating loaders (1956, IBM, GE, UNIVAC)
- allows multiple program segments (i.e. "control segments")
    - segments can be written in different languages
    - segments can be independently compiled
- tasks:
    - allocation:
        - assembler calculates segment size
        - loader calculates sum of sizes
        - OS determines load location
    - relocation:
        - assembler flags words for relocation (via bit-mask)
        - loader patches these records
    - linking:
        - handled loader via indirection

### Transfer vector (TV)
- contains 1 entry/external symbol used by this program segment
- assembler sets aside room at the start of each object file for the TV
- calls to external symbols are replaced with calls to locations in TV

### Disadvantages
- time and space overhead
- doesn't work for sharing data

### Sharing data
- permit one common shared data segment

## Relocation
- 2 kinds of relative:
    - relative to the LL of data segment
    - relative to the LL of control segment
- assembler must distinguish between them

## Direct linking loader
- very common linking/loading strategy
- pros:
    - separate assembly
    - multiple control/data segments
    - low overhead

## Assembler responsibilities
- header info
    - length of segment
    - execution start address
- list of entry points
    - global symbols defined in this segment
    - value of each of these symbols (relative)
- list of external references
    - global symbols used in this segment
    - value unknown at assembly-time
- relocation info
    - modification records
- machine code
    - text records

## Entry record
- similar to BSS loader
- list and define all entry points
- possible format: `N <symbol> <value>`
- conventions:
    - program name is always implicitly an entry point
    - entry points must be relative
    - handling absolute entry points is optional

## External record
- indicate which parts need to be patched with external symbols
- can be combined with text and modification records
- possible format: `T <address> <machinecode> X<size> <symbolname>`

## Pass 1
- 