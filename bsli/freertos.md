# FreeRTOS

[FreeRTOS Concepts](https://www.freertos.org/implementation/main.html)

## Capabilities
- the FreeRTOS kernel provides:
    - realtime scheduling
    - inter-task communication and synchronization primitives
- the FreeRTOS core libraries provide:
- the FreeRTOS plus libraries provide:

## Multitasking
- FreeRTOS is a multitasking kernel
- **task:** like CPU threads
- why multitask?
    - split code into smaller parts
    - easier testing
    - let OS handle complex timing/sequencing details

## Scheduling
- **scheduler:** the part of the kernel that decides when tasks can execute
- **scheduling policy:** the algorithm used to decide which task should execute at a given moment
    - differences between non-realtime and realtime kernels
- tasks can voluntarily suspend themselves:
    - **sleep:** wait for some time
    - **block:** wait for a resource/event (e.g. keypress, uart data, etc.)

## Context switching
- **context:** info that belongs to a task (its registers, memory, etc.)
- **context switching:** when a task is suspended, its context is saved. when a task is resumed, its context is loaded

## Ticks
- **tick:** time unit used by the kernel
    - ticks are driven by one of the STM32 clocks
    - each tick, the scheduler checks to see if any tasks should be suspended/resumed
- ticks are whole numbers, which has effects the accuracy of time delays (sleeping)
    - [Tick Resolution](https://www.freertos.org/tick-resolution.html)
- 
