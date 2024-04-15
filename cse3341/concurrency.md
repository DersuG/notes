# Concurrency

## Atomicity and determinism
- java has the `synchronized` keyword to ensure atomicity of multiple operations
- but this doesn't ensure determinism

```java
int x = 1;

/* thread 1 */
synchronized (m) {
    t = x;
    t = t + 1;
    x = t;
}

/* thread 2 */
synchronized (m) {
    t = x;
    t = t * 2;
    x = t;
}
```

## Rearranged code
- compilers and processors can rearrange code
```java
int data = 0;
boolean flag = false;

/* thread 1 */
data = 42; // this order is not necessarily guaranteed
flag = true;

/* thread 2 */
if (flag) {
    int t = data;
    print (t);
}
```
- volatile variables create memory barriers
    - but hurt performance

## Locks in Java
```java
int x = 1;

/* thread 1 */
while (!TAS(&m.lockBit, 0, 1)) {}
t = x;
t = t + 1;
x = t;
memory_fence; // prevents previous instructions being reordered past here
m.lockBit = 0;

/* thread 2 */
while (!TAS(&m.lockBit, 0, 1)) {}
t = x;
t = t * 1;
x = t;
memory_fence;
m.lockBit = 0;
```

## Data races
- 2 accesses to a variable, with at least 1 being a write
- one access doesn't happen completely before the other
- **roach motel:**
```java
data = ...; /* this can be moved by the compiler/cpu into the critical section, but not past */
acquire(m);
flag = true;
release(m);
```

## DRF0-based memory models
- modern **memory models** (via compiler+hardware) guarantee the following:
    - no data races mean **sequential consistency**
        - instructions appear to execute in an order that respects program order
    - data races mean possibly weak or undefined semantics!

## Detecting data races
- strategies:
    - vector clocks: check the happens-before relationship
    - collision analysis: check for conflicting accesses
    - lockset: assume and check locking discipline

### Vector clocks
- each thread maintains its own **logical clock** `c`
    - stores **logical time**
- initially, `c = 0`
- each time a synchronization release operation happens, `c += 1`
    - e.g. `unlock m`
    - e.g. volatile write
- the **vector clock** is a vector (array) of logical clocks
- an event (state of the vector clock) happened before another if each logical clock's value is <= the corresponding logical clock's value in the other event
- if some are greater and some are less than, it's an illegal state
![[Pasted image 20240403132708.png]]
- each thread stores an entire vector clock and updates the logical times of other threads' clocks through message-passing
![[Pasted image 20240403133040.png]]
- on each unlock, the thread stores a copy of the current thread's vector clock
    - the thread sends its vector clock to all other threads via message passing
    - then the thread increments its local clock
- on each lock, the thread fetches the vector clock stored in the lock
    - this takes O(n) time relative to the number of threads!
- on each write, the thread fetches the vector clock stored in the lock and compares it to its own vector clock
    - if there is no clear happens-before relationship between either vector clock, there is a data race (e.g. `[6 2]` vs. `[5 4]`)
- can only detect data races that actually happen
- will always detect a data race that happens
- 80x slowdown

### FastTrack
- improvement of vector clocks
- in a program with no data races, writes are properly ordered
- only need to track the last writer
- each thread has a vector clock
- on each write, the current thread's id and local clock is stored in the lock
- on each unlock, the current thread's entire vector clock is sent to other threads via message passing
- on each lock, fetch any inbound messages and compare their values against the thread id and clock stored in the lock

### Collision analysis
- try to make 2 conflicting accesses happen at the same time
- pause one thread at memory access to x
- catch other threads that access x
- can be automatic (instrumentation) or manual (added by programming)

### Lockset
- keeps track of locks associated with each thread and program variable
- on each lock, add the lock to the thread's lockset
- on each unlock, remove the lock from the thread's lockset
- if two accesses from different threads can happen with the locksets having no intersection, then there's a potential data race
- static analysis
- false positives

## Atomicity
- higher level property
- **atomicity:** operations appear to happen all at once or not at all
    - "power of the database"
- **serializability:** execution environment is equivalent to some serial execution of atomic blocks

```java
// no data races
// but not atomic

class Vector {
    synchronized boolean contains(Object o) {...}
    synchronized void add(Object o) {...}
}

class Set {
    Vector vector;
    /* synchronized */ void add(Object o) {
        if (!vector.contains(o)) {
            vector.add(o);
        }
    }
    int size() {
        return vector.size();
    }
}
```

## Stacks and heaps
![[Pasted image 20240412125002.png]]
![[Pasted image 20240412125017.png]]
![[Pasted image 20240412125032.png]]

## Concurrency exceptions
- java provides memory and type safety by handling buffer overflows, dangling pointers, array out-of-bounds errors, double frees, and some memory leaks by using exceptions
- should languages, runtime systems, and hardware provide **concurrency correctness?**
- general-purpose parallel software is a hard problem
- writing correct, scalable programs is hard

## More problems: double-checked locking
![[Pasted image 20240412130051.png]]
![[Pasted image 20240412130452.png]]
![[Pasted image 20240412130537.png]]
- `new` can assign before it actually initializes

![[Pasted image 20240412130611.png]]
