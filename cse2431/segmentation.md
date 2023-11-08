# Segmentation

## Paging issues
- OS helps with paging:
    - (process creation) create and initialize a page table, allocate space in swap area
    - (context switch) TLB flush, load new page table, (optional) prepaging
    - handling page faults
    - (process termination) release page table, pages, and disk space in swap area

### Shared pages
- saves memory space (e.g. shared libs)
- communication
- different page tables map some pages to the same physical frame
- issues:
    - releasing shared pages
    - swapping shared pages (pinning)
        - **page pinning:** inability to swap shared pages out unless all owners are also swapped out

### Copy-on-write (COW)
- to save the cost of memory copying, child processes share the entire memory contents of their parents
- pages are only copied when the child process writes
- question: should an OS schedule the parent or the child first after a `fork()`?

### Cleaning policy
- paging is great when there is a lot of free physical frames
- when should an OS start the page reclamation process?
    - (lazy policy) delay as long as possible, when all frames are in use
    - (via paging daemon) periodically or when free frames dip below a threshold

## Segmentation
- **segment:** logical unit
    - programmer is aware of them
    - e.g. code, heap, stack
- pros:
    - easy to handle dynamic expanding/shrinking data structures
    - linking is simplified if each procedure has its own separate segment
    - can share procedures and data
    - segments can have different protections
- segments live in physical memory
    - first-fit, best-fit, and worst-fit algorithms apply
    - external fragmentation

| consideration | paging | segmentation |
|---|---|---|
| programmer must know it's being used | no | yes |
| # linear address spaces | 1 | many |
| can total address space exceed physical memory? | yes | yes |
| can procedures and data be distinguished and separately protected? | no | yes |
| can tables with fluctuating sizes be accommodated easily? | no | yes |
| facilitates sharing of procedures | no | yes |
| why was it invented? | to get a large linear address space without more physical memory | to break programs and data into logically independent address spaces |

## Segmentation architecture
- logical address format: `[segment_number] [offset]`
- each process has a segment table
    - 1 entry for each segment:
        - `base`: starting physical address of segment
        - `limit`: segment length
        - other fields
- **segment table base register (STBR):** points to segment table's location in memory
- **segment table length register (STLR):** number of segments used by a program
- segment # *s* is valid if *s < STLR*
![[segmentation.png]]
![[segmentation_address_mapping.png]]
![[segmentation_sharing.png]]
![[segmentation_address_mapping_intel.png]]