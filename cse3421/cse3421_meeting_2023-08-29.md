# CSE 3421 Meeting 2023-08-29

## Performance metrics
- **response time (latency):** time between start and completion of task
- **throughput:** # of tasks completed in a time unit
- **resource consumption:** resources used to complete a task

## Performance
- `performance = 1 / execution_time`
- relative performance: `performance_a / performance_b = execution_time_b / execution_time_a = performance_ratio`

## Time units
- 1 millisecond (ms) = 10^-3 s

## Data size units
- 1 kilobyte (KB) = 2^10 bytes ~= 10^3

## Clock rate & cycle time
- `CC = 1 / CR`
    - **CC:** clock cycle (period)
    - **CR:** clock rate (frequency)
- higher transistor density means faster clock rate (?)

## CPU execution time
- `program_execution_time = clock_cycles_for_program * CC`
- `program_execution_time = clock_cycles_for_program / CR`
- can't improve `CC`, need to improve `clock_cycles_for_program`
- not all instructions are the same:
    - `clock_cycles_for_program = instructions_per_program * average_clock_cycles_per_instruction`

## Effective (average) CPI
- $\text{overall effective CPI} = \sum^n_{i=1} (\text{CPI}_i \cdot \text{IC}_i)$
    - `IC_i`: percentage of instructions of class i executed (frequency of that kind of instruction)
    - `CPI_i`:  cycles per instruction of class i
- CPI depends on architecture, can only compare on machines of the same architecture

## Bubble sort example
```c
int i, j;
for (i = 0; i < n - 1; i++)
    for (j = 0; j < n - i - 1; j++)
        if (arr[j] > arr[j + 1])
            swap(&arr[j], &arr[j+1])
```
- comparison and swapping have very different latencies
- 

## Performance equation
- `cpu_time = instruction_count * CPI * clock_cycles`
- `cpu_time = instruction_count * CPI / clock_rate`

## Compiler optimizations
- eliminate redundant instructions
- reordering instructions (to improve cache)
- choosing better instructions
- grouping data access
- etc.

## Reducing power/energy
- dynamic voltage-frequency scaling
- low power state for DRAM, disks (sleeping mode)
- overclocking - increasing clock rate of one core, but turning off other cores
