Note: You may notice there is not part 4. Part 4 is OS structures and not covered by this course.

# Part 5: Process

## 1. What is a process?
A process has the following features:
  - process ID (PID)
  - running state / status
  - process control block (records process information; like a log file; it's a data structure)
  - process scheduling
  - inter-process communication (IPC)

Note that _all_ PCB information is always stored in kernel space, even if the process is running in the user space.
  - stored in `/proc` (short for processes)
  - `% cd /proc` from anywhere

```
% ls -la
  1235  // a directory for a running process with pid 1235

% cd 1235
% ls -la
  stat status // these have size 0 since they are just a pointer to the PCB in the kernel space; therefor you cannot edit the information!
```

## 2. Process states
  - new
  - ready
  - running
  - waiting 
  - terminated
