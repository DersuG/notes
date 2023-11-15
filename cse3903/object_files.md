# Real Object File Format
- multiple segments
    - text (read-only) for instructions
    - data (read/write) for storage
- distinguish between R and RW segments:
    - to allow multiple processes to execute the same program using a shared text segment
- usually loaders group segment types together from multiple object files

## UNIX a.out

![[Pasted image 20231114174407.png]]

### Header structure

All object files begin the same way:

```c
int a_magic;  /* magic number */
int a_text;   /* text segment size */
int a_data;   /* data segment size */
int a_bss;    /* uninit data size */
int a_syms;   /* symbol table size */
int a_entry;  /* entry point */
int a_trsize; /* text relocation size */
int a_drsize  /* data relocation size */
```
- int is 4 bytes, so header is 32 bytes

### Text and data records
- similar to our text records

### Relocation records
- handles both relocation and external symbols
- one 8-byte entry for each location to be patched
    - 4 bytes: address
    - 3 bytes: index
    - 1 byte: flags
- address: location (offset within segment) to patch
- flags:
    - 2 bits for length of patch item (1, 2, 4, or 8 bytes)
    - 1 bit ("extern bit") to indicate reason for patching
        - 1=external
        - 0=relocation
- index: meaning depends on extern flag
    - 1=index is symbol number from symbol table
    - 0=index indicates segment (text, data, bss, etc)

### Symbol table
- each 12-byte entry describes a symbol
    - 4 bytes: name offset
    - 4 bytes: type/spare/debug info
    - 4 bytes: value
- name offset: pointer into string table
    - allows arbitrarily long names
- type byte: low bit is "external" (global) flag
    - text/data/bss: relative symbol to that segment
    - abs: absolute value (may or may not be external)
    - undefined: external bit must be on

### String table
- one long string of null-terminated substrings

## Evolution of object file formats
- **magic number:** distinguish file types
    - pdf: `25 50 44 46` (`"%PDF"`)
    - gif: `47 49 46 38 39 61` (`"GIF89a"`)
    - png: `89 50 4E 47 0D 0A 1A 0A` (nonascii + `"PNG"` + newlines to detect conversion problems + EOF to halt output on MSDOS)
    - `A.out`: `04 07` (PDP-11 machine instruction for unconditional branch past header info)
- obsolete object format variations: zmagic, qmagic, etc.
- **executable and linking format (ELF):**
    - magic number `75 45 4c 46` (`".ELF"`)
    - better support for shared libs
    - position independent libraries
- `hd [FILE]` or `hexdump -C [FILE]`
- `readelf -a [FILE]`