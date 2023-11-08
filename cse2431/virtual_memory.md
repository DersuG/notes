# Virtual Memory
- artificially increases ram
- isolates programs
- **virtual memory:** memory that applications see
- **paging:** the process of moving memory from disk into virtual memory
    - **page table:** tracks which hardware memory pages map to which virtual memory pages
        - processes have their own page tables
- **frame:** locations in real memory where virtual pages are mapped to
- logical addresses are divided into page numbers and page offsets (just like cpu cache)
- **page table base register:** tracks location of page table for each process
    - must be changed during context switches

## Multilevel page tables
- like multilevel cache
- page number: `[idx_pt1] [idx_pt2]`
    - index into a page table of page tables
    - index into that second level page table
- takes less ram than a large monolithic page table
- page tables themselves can be on disk
- performance hit because of multiple memory accesses

## Typical page table entry
- `[is_caching_disabled] [is_referenced] [is_modified] [protection] [is_present/absent] [page_frame_number]`

## Demand paging
- page table acts as a cache in ram for data on disk
- **page fault:** when an address is on a page that isn't in the page table
- **page fault handler:** swaps pages in and out of the page table
    - in vm subsystem of the os

## TLB
- improves performance of page tables
- caches a page so that repeated accesses in the same page can bypass the page table
