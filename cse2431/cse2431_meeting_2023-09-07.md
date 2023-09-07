# CSE3421: Meeting 2023-09-07

## Threads
- share:
    - process info (parent, time, etc.)
    - memory (segments, page table, etc.)
    - I/O and files (serial ports, directories, file descriptors, etc.)
- don't share:
    - state (ready, running, blocked, etc.)
    - registers
    - program counter
    - stack
- implementations vary
    - some kernels aren't aware of threads
    - 
