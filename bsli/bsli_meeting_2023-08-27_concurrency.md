# Concurrency
## Context
- unofficial meeting led by Toby Simpson as a crash course on concurrency
- important because we will likely be using FreeRTOS's multitasking capabilities for the flight computer
- we want to know:
    - what kinds of problems we will face
    - what is and isn't possible/sane with multitasking
    - how we will structure our tasks and data flows

## Threading
- models the interleaving of instructions from different processes
- shared code and data

## Processes
- don't share code and data
- can have multiple threads

## Shared memory
- ways to talk between processes

## Producer consumer problem
- shared buffer
- producer thread enqueues
    - produce item
    - wait for next buffer spot to empty
    - put item in buffer
    - move producer down buffer
- consumer thread dequeues
    - wait for buffer to have an item
    - remove item
    - move consumer down buffer

### Synchronization
- don't waste space
- track # of items in buffer
- producer increments count
- consumer decrements count
- must be atomic to prevent race condition, or use semaphores

### Bounded buffer
- consumer
    - wait if buffer has no items
    - wait if producer is using buffer
    - remove item
    - signal producer you're done with buffer
    - signal producer you removed an item
- consumer
    - produce item
    - wait if buffer has no empty spots
    - wait if consumer is using buffer
    - put item in buffer
    - signal consumer you're done with buffer
    - signal consumer you added an item

## Semaphores
1. wait for critical section to open up
2. enter critical section
3. signal to others that you're done with critical section

## Reader writer problem
- shared data set
- readers can only read data
- writers can  read and write data
- want: allow multiple readers to read at the same time, but only allow one writer to write at a time
    - count of currently reading readers
- reader
    - wait for readers to be finished with the count
    - increment count
    - if you're the first one, wait for any writers to finish
    - signal other readers you've finished incrementing
    - read data
    - wait for any readers to be finished with the count
    - decrement the count
    - if you're the last reader, signal the writers you're finished
- writer
    - wait for readers and writers to finish
    - write
    - signal to other writers that you're done

## Conclusion
- the division of labor that multitasking gives us is worth it
    - we can split the problem over different programming teams
- we should try to keep things simple to prevent bugs
- proposed architecture:
    - **sensor control task:** reads from sensors, sends data to other tasks that need it
        - also performs telemetry (may be split into its own task later on if things go well)
    - **airbrake control task:** controls airbrakes, needs to get data from sensor control task
    - **recovery control task:** does recovery stuff, needs to get data from sensor control task
        - internally keeps track of flight plan/state (boost, coast, apogee, drogue, main, land)
