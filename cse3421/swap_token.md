# Swap-Token: an OS Solution for Memory Thrashing in Multi-Programming Environment

## Memory management for multiprogramming
- OS's page replacement manages physical memory sharing among different processes
- global LRU replacement is commonly used in the entire memory space
- **thrashing:** when accumulated memory requests from multiple programs exceeds available user space
    - no program can establish its own working set
    - causes tons of page faults
    - low CPU use, waiting for pages to swap
    - execution of each program practically stops

## Thrashing protection methods
- local page replacement
    - each program is statically allocated a fixed size
    - DEC VAX machine had this in its VMS in early 1980s
- load control
    - while thrashing, some jobs are suspended/swapped
    - OpenBSD, IBM RS/6000, HP9000
    - linux makes load controls based on RSS (resident set size) to limit the data allocation for a process with high RSS
    - RSS is used to measure the % of pages in memory of a process

## Load control problems
- thrashing is often triggered by short spikes of memory demands
    - a load control can overreact
- job suspension also quits other stuff
    - restarting jobs is expensive
- want: a lightweight and dynamic protection

## Thrashing
- the global LRU replacement generates 2 types of LRU pages for replacement:
    - true LRU pages
        - programs don't need access
    - false LRU pages
        - programs haven't been able to access them yet because the required working set isn't set up yet, or page faults are happening
        - amount of false LRU pages indicates how bad thrashing is
    - systems can't distinguish between them

## Token-based thrashing protection
- Jiang/Zhang, Performance Evaluation, 05.
- sudden spikes of memory demand from one job can generate many false LRU pages in others, particularly in less demanding ones
- even with lots of physical memory, as false pages reach a certain amount, the system slows to a crawl
- basic idea: as the system enters a pre-thrashing stage (low RSS and high idle CPU), a token is issued to a process so that it can quickly form its working set and proceed
- this approach can effectively and timely avoid thrashing
- implementation details:
    - which process should be issued a token?
        - processes demanding less memory
    - how long do processes hold the token?
        - adjustable and proportional to the thrashing degree
    - what happens if thrashing is too serious?
        - becomes a polite load control mechanism by setting long token times so programs are executed one by one
    - multiple tokens can be effective for light thrashing

## Evolution
- first version:
    - token is randomly given to process
    - timestamp used to hand over the token one by one
    - limitation: tokens may not be given to most desirable jobs
    - limitation: a constant timestamp may not address urgency
- preempt swap token:
    - "priority counter" set for each process to count the swapped-out pages
    - higher = more help needed
    - counter incremented for a unit of swapped-out pages
    - token given to process with high count ("priority")
    - length of timestamp varies by priority degree

## Impact
- it's in the linux kernel
