# FAST Data Accesses in DRAM
- invented by Robert Dennard in 1968
- cheaper than SRAM
    - 1 transistor and capacitor per cell vs. 6 transistors per cell
    - capacitors must be periodically recharged
- slower than SRAM

## DRAM locality
- DRAM is the center of memory hierarchy
    - high density and capacity
    - cheap and fast accesses (compared to disks)
- cache misses are usually considered to be constant delays
    - DRAM has non-uniform access latencies!
- **row-buffer:** a fast cache mechanism in DRAM
- **buffer cache:** ??

## Memory banks
- pages of memory are moved around the memory hierarchy
- DRAM is divided into different boards (ranks), each with several banks (chips)
- **rank:** DRAM board
- **bank:** memory chip on DRAM board
- e.g. a 2GB memory module with 16 128MB banks

## DRAM latency
- access steps:
    - precharge (pre-charge capacitors)
    - row access (find row in row buffer)
        - find row in DRAM core on miss
    - column access (find column in row)
    - send to memory bus
- `access_time = latency + bus_bandwidth_time`

## DRAM pre-charge
- aka "recharge" or "refresh"
- each cell must be precharged every 64ms
- a row read/write automatically precharges the row
    - row buffer is frequently and automatically precharged
    - row buffer in modern DRAM is built by SRAM (?)
- every precharge operation covers several rows
- memory controller issues precharges

## Nonuniform DRAM access latency
- case 1: row buffer hit (20+ns)
    - column access
- case 2: row buffer miss, row is precharged (40+ns)
    - row access
    - column access
- case 3: row buffer miss, row is not precharged (~70ns)
    - precharge
    - row access
    - column access

## Amdahl’s law applies in DRAM
- as bandwidth improves, DRAM latency becomes the bottleneck
- DRAM latency will decide cache miss penalty

## Page interleaved memory
- each page is assigned to a different bank
    - (e.g. pages 0 and 4 on bank 0, pages 1 and 5 on bank 1, pages 2 and 6 on bank 2, and pages 3 and 7 on bank 3)
- address format: `[page_index] [bank_index] [page_offset]`
- on a row buffer hit:
    - if bank index is the same, page indices stay the same (same page)
    - if bank index is different, page indices stay the same (concurrent accesses)
- on a row buffer conflict:
    - if bank index is the same, page indices are different (to keep replacing the row buffer (access different pages in the same memory bank))
    - if bank index is different, page indices are different (to keep replacing the buffer in different banks)

### Correspondence in cache and DRAM conflicts
- **address conflict:** when multiple addresses map to a cache line
- on a cache conflict: same cache index, different tags
- on a row-buffer conflict: same bank index, different pages
- for any 2 addresses, if they conflict on the cache, then they will also conflict on the row-buffer!
- **symmetry:** invariance (of conflicts) in results under a transformation (of the conventional memory mapping).
    - Address mapping symmetry propagates conflicts from the cache address pace to the memory address space
    - cache conflicting addresses are also row-buffer conflicting addresses
        - cache conflict misses are also row-buffer misses
    - cache write-back conflicts
        - dirty cache lines need to be written to DRAM
        - memory controller brings that page to row buffer by evicting the existing page causing row-buffer miss on the existing page

### Write-back conflicts
- when a dirty cache block (A) is replaced by another block (B), it needs to be written to DRAM
- page A will be loaded into the row-buffer, replacing the current page in the row-buffer
- 

- when a dirty cache block is evicted, it will be written to DRAM
- the page will loaded into the row-buffer, replacing the current page in the row-buffer
- 

## Permutation-based page interleaving
- breaks the symmetry
- new bank index is determined by xor of some bits in cache tag and the lower bits of the bank index
    - doesn't change memory addresses, only changes bank index
- conflicting addresses are distributed into different banks
- spatial locality within each page is preserved
- pages are uniformly mapped onto all banks
