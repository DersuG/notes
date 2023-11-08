# Deadlock

## Resource
- **resource:** something needed by a process
    - **serially reusable:** cpu, memory, disk space, io devices, files, etc.
    - **consumable:** messages, info, interrupts, etc.
    - **preemptible:** cpu, physical memory, etc.
    - **non-preemptible:** tape drives, etc.
    - **shared:** multiple processes can share it
    - **dedicated:** exclusive to one process

## Deadlock
- a processes is **deadlocked** if it's waiting for something that will never happen
    - typically involves multiple processes (the "deadly embrace")
- different from starvation:
    - in process starvation, the process can logically proceed but the system never gives it resources

### Deadlock conditions
- necessary and sufficient conditions:
    - mutual exclusion (processes have exclusive control of resources)
    - **wait-for condition:** processes hold resources allocated to them while waiting for additional resources
    - **no preemption condition:** resources can't be removed from the process until voluntary release at the completion of resource use
    - **circular wait condition:** circular chain of processes, each holding resources requested by the next process

### Resource allocation graph

### Handling deadlocks
- prevention
- avoidance
- detection
- recovery

## The Ostrich algorithm
- stick ur head in the sand!!
- don't do anything, simply restart the system
- makes common path faster and more reliable
    - deadlock prevention, avoidance, or detection/recovery algorithms are expensive
    - if deadlocks occur rarely, it's not worth the overhead
- what linux does

## Locking with 2 phases
- lock everything you need, 1 at a time
- if you can't acquire a resource, unlock everything and start over
- only once you have everything can you begin
- breaking a circular wait condition:
    - global ordering of resources

## Deadlock detection
- wait-for graphs
- 1 resource per type
- check for cycles

## Deadlock avoidance: banker's algorithm
- system needs to know resources ahead of time
- each customer tells the banker the max amount of resources it needs
- customer borrows resources
- customer returns resources
- customer eventually pays back loan
- banker only lends if system will be in a safe state after
- **safe state:**
    - there exists a lending sequence that lets all customers take out a loan
    - there's some scheduling order such that all processes can run to completion even if they all suddenly request their max amount of resources
- **unsafe state:**
    - there's a possibility of deadlock
    - some processes could still complete
