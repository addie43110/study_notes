# Study notes for CSC 360 Midterm 1

## Part 1: Review of computer organization
1. [Computer organization] (#1-computer-organization)
2. [CPU] (#2-cpu-central-processing-unit)
3. [Memory] (#3-memory)
4. [I/O] (#4-inputoutput)
5. [Different architectures] (#5-different-architectures)

### 1. Computer organization
TODO: Add diagram

### 2. CPU (Central Processing Unit)
  - made up of:
    - ALU (Arithmetic + Logic Unit)
    - PC (program counter)
    - level 1 cache
    - registers
    - clock
    - FPU (floating point unit)

### 3. Memory
* Note that network storage is difficult to compare right now. It is normally slower, but in the future, it could easily be faster than a CD.
TODO: Add diagram


### 4. Input/Output
#### 4.1 Access
  - need to give address (port #, what interrupt, pin numbers)
  - need to know if we're using interrupts or DMA (direct memory access)

#### 4.2 Synchronous vs asynchronous
TODO: add diagram

#### 4.3 Interrupt vs trap
  - interrupt: external control stops a process
  - trap: interally stop a process, like an exception (kind of like a try-catch in Java)

#### 4.4 Direct memory access (DMA)
  - for transferring large sizes of data, it is wasteful to ask the CPU to do the job
  - special type of processor
  - the job for the CPU is to tell the DMA the source address of the data, the destination address for the data, and the size of the data

### 5. Computer architectures
  - single-processor systems (single-CPU)
  - multi-processor systems
    - OS determines which CPU is designated which job
  - cluster systems
    - multiple computers in one area
  - distributed systems
    - different computers in multiple areas using internet to connect

## Part 2: Overview of Operating Systems
1. [History] (#1-history)
2. [Batch processing vs time sharing] (#2-batch-processing-vs-time-sharing)
3. [Main tasks of OS] (#3-main-tasks-of-os)

### 1. History
  - requirements change
    - before, computers were very expensive, thus the goal was to figure out how to efficiently use a computer.
    - the answer was to share computers
    - now, users have become more expensive (our time is expensive), so the goal is no longer to effectively use a computer.
    - the result is to share users among multiple computers

#### 1.1 Share computers
  - put a limit: max # of processes a user can run
  - use % ulimit -u to check this limit (and avoid fork() bombs!)

#### 1.2 Generation of OS
  - before: uniprogramming (one program can run at one time)
  - now: multiprogramming (run multiple programs at same time)

### 2. Batch processing vs time sharing
Batch processing:
  - load a pool of jobs
  - execute one job until it terminates or it is blocked (for eg. accessing I/O)
  - pick next job to run (decided by job scheduler)

Time sharing:
  - execute one job up to a certain time (time slice)
  - switch to another job)
  - switch decided by CPU scheduler


### 3. Main tasks of OS

#### 3.1 Process management
  - create, delete, suspend, resume, and schedule processes
  - synchronization
  - IPC (inter-process communication)

#### 3.2 Memory management
  - store/access data in main memory
  - keep track of available space in main memory
  - allocate and reclaim main memory
  - how to swap in and out of virtual memory


#### 3.3 Storage and I/O management
  - file systems
  - printer, network devices (device driver) (we will not touch on this)
