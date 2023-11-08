# Virtual Memory
- virtual memory pages can be assigned to any **page frame** of physical memory
- **TLB:** caches translations
- page allocation:
    - contiguous (Pn)
    - non-contiguous (P1)

## Address translation
- virtual address: `[virtual_page_num] [page_offset]`
- no need for tags because each process has its own table
- virtual address page number is translated to a physical page number
- physical address: `[physical_page_frame_num] [page_offset]`
- there can be more virtual addresses than physical addresses
    - page frame will have less bits

## Page table
- want: fully associative page placement
- **memory management unit (MMU):** handles the page table, address translation, and TLB
- each process has its own
- structure:
    - `[valid:1] [access_rights:?] [physical_page_frame_number:?]`
    - virtual page number gets the row
    - **valid bit:** 0 = in disk, 1 = in memory
    - **access rights:** none, read only, read/write, execute
- **swap space:** space on disk for pages
- OS also tracks page accesses for use in LRU or LIRS, etc.

## Page table register
- each process has one
- **page table base register (PTBR):** points to start of page table
    - for TLB, each process gets an **address space number (ASN)**
        - ASN register is like your ID

## Page faults
- page fault is like cache miss
- if valid bit = 0, physical page number points to a location on disk
- new processes start with all pages having valid bit = 0 (all pages on disk)
- **demand paging:** pages are only loaded into memory when needed

## Translation lookaside buffer (TLB)
- cache for page table translations
- virtual memory needs 2 memory accesses:
    - translate virtual address to physical address (page table is in physical memory)
    - transfer the actual data
- observation: locality of data pages means locality of their virtual addresses
- **address space number (ASN):** an ID for the address space of a process
    - used by TLB to cache page table entries of multiple processes
- format: `[virtual_page_number(PID/"tag")] [physical_page_frame_number("address")] [valid:1] [ref:1] [dirty:1] [access_rights:?]`
    - **dirty bit:** for write-backs
        - when replacing a page, need to know if it needs to be written to disk first
    - **ref bit:** used to calculate LRU on replacement
        - set when page is accessed
        - OS periodically sorts and moves referenced pages to the top and resets all ref bits
- can be fully associative, set associative, direct-mapped
- usually small, typically 32-512 entries

## TLB hit vs. miss
- hit:
    - CPU issues virtual address
    - MMU checks TLB
    - TLB hit
    - MMU accesses physical address
    - page is sent to CPU
- miss:
    - CPU issues virtual address
    - MMU checks TLB
    - TLB miss and MMU accesses page table
    - physical address is sent to MMU
    - TLB is updated
    - MMU accesses physical address
    - page is sent to CPU

## Virtual caches vs. physical caches
- in a virtual cache, the TLB uses the virtual address
    - TLB is accessed before MMU
- in a physical cache, the TLB uses the physical address
    - TLB is accessed after MMU
    - discussed in this class

