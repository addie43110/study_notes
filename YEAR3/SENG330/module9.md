# Concurency
- aspect of the problem domain
- handle multiple simultaneous (or near simultaneous) events
- note: parallelism is different
  - aspect of solution domain
  - want to make program run faster by processing different portions of the proble in parallel

## Threads and Processes
- process: self-contained, no (directly) shared memory
  - synchronization via message passing
- thread: lightweight process, shared memory
  - easy resource contention problems
  - synchronization via shared memory

## Threads in Java
Two main problems with thread synchronization:
1. thread interference
  - steps of competing threads overlap

```java
class Counter {
  private int c = 0;
  
  public void increment() { c++; }

  public void decrement() { c--; }

  public int valu() { return c; }
}
```

- remember that `c++` or `c--` compiles into something more like

```java
int temp = c;
c = tmp + 1;
```

- and beyond that, assembly code gets broken up into load, increment and store commands

2. memory consistency errors
  - series of operations need to be linearizable (or sequentially consisten)
  - operations are atomic
  - can map to real-world consistent behaviour


## Prevent concurrency problems
One way to prevent concurrency problems is to think about the following properties:
1. Mutual exclusion
  - only one thread in critical section at one time
2. Making progress
  - if no one is in the critical section, the one who wishes to enter should be able to
3. Bounded waiting time
  - threads waiting to get into the critical section must not wait forever

## Synchronization in Java
- use keyword `synchronized`
- whole method body becomes "locked"

```java
synchronized T m() {
  /* whole body is critical section */
}
```

- every call to `m()` must:
  - acquire lock
  - execute `m()`
  - release lock

In order to just lock a certain section / block of your code, use `synchronized(this)`

```java
synchronized(this) {
  /* critical section */
}
```

## When is concurrency useful?
- abstraction
  - separating different tasks
  - don't worry about when to execute them
  - or execute them at "same time"
- responsiveness
  - good for user interfaces
  - different tasks execute independently
  - ex. download something while also continuing to browse internet
- performance
  - split complex tasks into smaller units
  - assign each unit to a different processor

## OO Concurrency principles
- always lock during updates to object fields
- always lock access of possibly updated object fields
- never lock invocation of methods on other objects
