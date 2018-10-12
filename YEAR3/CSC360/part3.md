# Part 3: Interfaces to OS services

## 1. Operating system services
  - dual mode operation
    - mode bit is set to 1 when user is in the user space
    - mode bit is set to 0 when running in kernal mode
  - system calls

### 1.1 `open()` vs `fopen()`
  - `open()` directly calls system functions
    - need to include `#include <sys/types.h>`
    - then goes to kernel implementation
  - `fopen()` is a part of the C library, which then calls `open()` amongst other things
    - calls standard C library
    - the library in turn calls system call interface
    - then goes to kernel implementation
  - use `% man` to tell which functions belong to C vs system libraries (short for manual)

### 1.2 Command line interface
  - `ls`
  - `man`
  - `ps` - report a snapshot of the current processes
  - `ulimit`


