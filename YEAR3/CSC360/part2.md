# Part 2: Overview of Operating Systems
1. [History] (#1-history)
2. [Batch processing vs time sharing] (#2-batch-processing-vs-time-sharing)
3. [Main tasks of OS] (#3-main-tasks-of-os)

## 1. History
  - requirements change
    - before, computers were very expensive, thus the goal was to figure out how to efficiently use a computer.
    - the answer was to share computers
    - now, users have become more expensive (our time is expensive), so the goal is no longer to effectively use a computer.
    - the result is to share users among multiple computers

### 1.1 Share computers
  - put a limit: max # of processes a user can run
  - use % ulimit -u to check this limit (and avoid fork() bombs!)

### 1.2 Generation of OS
  - before: uniprogramming (one program can run at one time)
  - now: multiprogramming (run multiple programs at same time)

## 2. Batch processing vs time sharing
Batch processing:
  - load a pool of jobs
  - execute one job until it terminates or it is blocked (for eg. accessing I/O)
  - pick next job to run (decided by job scheduler)

Time sharing:
  - execute one job up to a certain time (time slice)
  - switch to another job)
  - switch decided by CPU scheduler


## 3. Main tasks of OS

### 3.1 Process management
  - create, delete, suspend, resume, and schedule processes
  - synchronization
  - IPC (inter-process communication)

### 3.2 Memory management
  - store/access data in main memory
  - keep track of available space in main memory
  - allocate and reclaim main memory
  - how to swap in and out of virtual memory


### 3.3 Storage and I/O management
  - file systems
  - printer, network devices (device driver) (we will not touch on this)
