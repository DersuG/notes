# Synchronization

## Critical section
- multiple threads/processes compete for shared data
- **race condition:** 2+ threads/processes access shared data and the final result depends on who runs precisely when
- **critical section:** code where shared data is accessed
    - need to ensure that nobody else is in a critical section
- critical section properties:
    - **mutual exclusion:** no other process must execute its own critical section while we are in ours
    - **progress:** if no process is accessing the resource but several want to, we can't be stuck
    - **bounded wait:** to access your critical section, there should be a bound on how long to wait for other processes to enter/leave the critical section
    - **speed and cpu count:** can't assume anything about either
- `block_time = access_time - request_time`

## Busy-waiting

### By disabling interrupts
- disable interrupts before entering critical section
- enable them after leaving
- this blocks switching

### With lock variables
```c
int lock;
lock = 0;

void thread(void) {
    while (lock) {
        lock = 1;
        /* critical section */
        lock = 0;
    }
}
```
- this is... questionable

### Strict alternation
```c
void thread(void) {
    while (true) {
        while (turn != my_thread_id) {}
        /* critical section*/
        turn = other_thread_id;
        /* other work */
    }
}
```
- satisfies mutual exclusion
- doesn't satisfy progress
- **spin lock:** a lock that uses busy waiting

### Using flags
```c
int flag[2] = {false, false};

int thread(void) {
    while (true) {
        flag[my_thread_id] = true;
        while (flag[other_thread_id]) {}
        /* critical section */
        flag[my_thread_id] = false;
        /* other work */
    }
}
```
- can block infinitely

### With Peterson's solution
```c
int flag[2] = {false, false}
int turn;

void thread(void) {
    while (true) {
        flag[my_thread_id] = true;
        turn = other_thread_id;
        while (flag[other_thread_id] && turn == other_thread_id) {}
        /* critical section */
        flag[my_thread_id] = false;
        /* do other work */
    }
}
```
- this works

### With test & set (TSL)
- needs hardware support
- does test and set atomically
```c
char test_and_set(char *target) {
    char temp = *target;
    *target = true;
    return temp;
}
```
- if the returned copy is zero, then the lock was set
- similar hardware instruction:
```c
void swap (char *x, char *y) {
    char temp = *x
    *x = *y;
    *y = temp;
}
```

## Sleep and wakeup
- alternative to busy-waiting
- when blocked, a process sleeps
- wake when it's ok to retry entering the critical section
- semaphore operation executes sleep/wakeup

## Semaphore
- counts the # of abstract resources
- operations are atomic
- **wait operation (Down, P):** acquire resource, decrements count
- **signal operation (Up, V):** release resource, increments count
```c
void wait(struct semaphore *s) {
    s->value --;
    if (s->value < 0) {
        /* add this process to s->list */
        block();
    }
}

void signal(struct semaphore *s) {
    s->value++;
    if (s->value <= 0) {
        /* remove a process p from s->list */
        wakeup(p)
    }
}
```

## Mutex
- binary semaphore
- variable with 2 states: lock, unlock

## Tradeoffs
- busy-waiting (spinlock)
    - wastes cpu
- sleep & wakeup (blocked lock)
    - context switch overhead
- spinlocks ok if waiting time is shorter than context switch time
- use sleep & wakeup otherwise

## Monitors
- a simpler way to synchronize
- a set of programmer-defined operators
- internal implementation can't be accessed directly
- encapsulation means it can access local variables only with local procedures
- doesn't allow concurrent access to the procedures
- only 1 thread/process can be active within the monitor at a time
- synchronization is built in

## Barriers
- usage:
    - processes approach barrier
    - all but one blocked at barrier (?)
    - last arrives, all are let through
- problem: wastes cpu if workloads are unbalanced
- implementation:
    - for N processes: with messages
    - for N threads: using shared variables

## Classic problems

### Producer-consumer problem
- producer adds items to buffer
- consumer takes items from buffer
- finite buffer

### Reader-writer problem
- readers read data
- writers write data
- rules:
    - multiple readers can access data at once
    - only 1 writer can write at once
    - a reader and writer can't be in critical section concurrently
- **locking table:** whether any 2 can be in the critical section simultaneously

### Dining philosophers' problem
- philosophers sit in a circle, with 1 fork between each pair
- philosophers eat or think
- eating needs 2 forks
- must pick up 1 fork at a time
- how to prevent deadlock?
