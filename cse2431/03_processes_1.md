# Processes 1

## Process
- an executing instance of a program
- each process has its own context

## Process control block (PCB)
- contains info for a process:
    - process state
    - PID
    - program counter
    - CPU registers
    - CPU scheduling info
    - memory-management info
    - accounting info
    - I/O status
    - parent PID

## Process states
- processes change state while running:
    - **new:** being created
    - **running:** is running
    - **waiting (blocked):** is waiting
    - **ready:** waiting to be assigned to a CPU
    - **terminated:** done
- 
