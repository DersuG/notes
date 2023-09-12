# CSE 2431: Meeting 2023-09-12

## Quiz 1
- when a process gets switched off the CPU, but hasn't finished its task, it transitions to the **ready** state
    - the wait state (aka block state) happens when the process is waiting for a resource

## Thread implementations
- user-level
- kernel-level
- hybrid

## User-level threads
- process table in kernel
- thread table in user library
- switching thread contexts is fast, because it's entirely within user level
- if a thread blocks, the whole process blocks

## Kernel-level threads
- both process table and thread table in kernel
- switching thread contexts is slow, because we have to switch from user level to kernel level

## Hybrid threads (scheduler activations)
- multiplex user-level threads into some kernel-level threads
- **virtual processors:** kernel-provided threads
    - multiple user-level threads can be matched to one virtual processor
- kernel-level scheduler talks to user-level scheduler to dynamically match user-level threads to virtual processors, creating new virtual processors as needed

## Threading problems
- global variables are shared (e.g. `errno` gets changed before it can be checked)
    - possible solution: each thread gets its own global variables
- library functions may not be re-entrant
