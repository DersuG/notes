# Scheduling
- https://en.wikipedia.org/wiki/Scheduling_(computing)

## Ready queue
- where processes wait to get cpu time
- **arrival time:** when a process enters the ready queue
    - changes the waiting time!

## First come first serve (FCFS) algorithm
- non-preemptive
- run each progress completely before running other processes, in the order they arrived
- `px_wait_time = px_start_time - px_arrival_time`
- `avg_wait_time = (p1_wait_time + p2_wait_time + p3_wait_time + ...) / num_processes`
- only switch off processes when they do IO operations
    - they get moved into the IO queue
- used by batch systems - susceptible to convoy effects

## Convoy effects
- when many IO-bound processes and 1 CPU-bound process arrive
- IO-bound processes pass through ready queue and go to IO queue
- CPU-bound process runs on the cpu until completion
- IO-bound processes are forced to wait

## Shortest job first (SJF) algorithm
- https://en.wikipedia.org/wiki/Shortest_job_next
- non-preemptive
- processes are arranged in the ready queue according to their estimated processing time
- need to estimate processing time
- also used by batch systems
- susceptible to process starvation (one long process is continuously blocked by many short processes)

## Round robin algorithm
- processes get a fixed amount of cpu time
- high overhead, especially for a small time unit
- waiting times are longer
