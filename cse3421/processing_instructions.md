# CSE3421 Meeting 2023-09-05

## Processing instructions
- instructions encode other info, like source/destination/operand registers, addresses, offsets, etc.
- **immediate values:** constants used in instructions
    - approaches:
        - put typical constants in memory and load them
        - make hard-wired registers (e.g. MIPS `$zero`)
        - have special instructions that contain constants (e.g. MIPS `addi`)

## Byte-addressable memory
- **byte addressable:** each memory address points to a byte
    - most architectures
- `num_addressable_bytes = 2^address_size`
- why bytes?
    - 8-bit ASCII, with 128-255 for special characters (e.g. European languages)
- registers aren't addressed by memory addresses
- **big-endian:** MSB first
- **little-endian:** LSB first
- **alignment restriction:** memory address of words must be on word boundaries (e.g. a multiple of 4 in MIPS)

## Signed integers
- `max = 2^31 - 1`
- `min = -2^31`
- to negate in 2's complement:
    - flip all bits, then add 1
    - `-x = ~x + 1`
