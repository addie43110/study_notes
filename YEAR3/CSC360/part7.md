# Part 7: Threads

## 1. What are threads?
  - reduce the number of context switches (which are costly and waste CPU time)
  - every thread has its own running environment
    - own stack
    - own registers
  - each thread shares
    - process code
    - process data
    - process files
The operating system has no control over which thread is run, only processes.
Q. Is there any way to designment more CPU time to a specific thread? There is, but we need more mechanisms to do this.
  

### 1.1 Motivation and benefit
  - if we create a thread instead of a forking to create a new process, we only have 1 PCB

## 2. User threads vs kernel threads
If we link a kernel thread to user thread, we can implicitly control how much time is given to the user thread. This is highly OS dependent.

## 3. Thread model: How Solaris implements threads
TODO: add diagram

### Light-weight process (LWP)
  - an intermediate data structure between user threads and kernel threads
  - from the user thread's point of view, LWP appears to be a virtual processor (like a virtual CPU) on which the application can schedule a user thread to run

### Example
Assume the system has only 1 physical CPU. Assume we have 3 kernel threads and a CPU Round-Robin (take turn) schedule. The user space has three processes such that P1 has three threads, P2 has 2 threads, and P3 has one thread. Each process has one LWP which also uses Round-Robin. Thus, P1 threads each get 1/9 CPU time, P2 threads each get 1/6 CPU time and the P3 thread gets 1/3 CPU time.
TODO: add diagram

## 4. Thread issues
Q. What happens when a thread calls `fork()`?
A. Either all threads are copied, or it will only copy the calling thread. This depends on the OS.
<br/>
If you are running Solaris (before version 10) and use Solaris threads API (instead of POSIX thread API), `fork()` will duplicate ALL threads in the process. But OS provides another function, `fork1()` to duplicate the calling thread only.
<br/>
If you are using the POSIX thread API, no matter whether `fork()` is called in the process of in the thread, only one thread will be copied into a new process.

### Signal handling
Q. Where should a signal be delivered?
A. Options
  - deliver the signal to the thread to which the signal applies
  - deliver the signal to every thread in the process
  - deliver the signal to certain threads in the process
  - assign a specific thread to receive all signals for the process

## 5. Pthread
  - `pthread_attr_init(&attr);`
  - `pthread_create(&tid, &attr, runner, NULL);`
  - `pthread_join(tid, NULL);`
  - `pthread_exit(0);`
  - `#include <pthread.h>`

