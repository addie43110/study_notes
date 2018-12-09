# Interfaces to OS Services

## `fork()`
```c
pid_t fork();
```

**Return value**

On success:
  - PID of child process is returned in the parent
  - 0 is returned in the child

On failure:
  - -1 is returned in the parent
  - no child process created
  - errno is set

### Resource sharing
How are resources allocated to a child process?
  - it depends on the OS
  - the child process may obtain its resources directly from the OS (gets a new PCB)
  - the child process may be constrained to a subset of resources of the parent process (does not get a new PCB)


## `wait()`
```c
pid_t wait (int *status);


pid_t waitpid (pid_t pid, int *status, int options);
```

The `wait()` system call suspends execution of the calling process until one of its children terminates. The call `wait(&status)` is equivalent to:
```c
waitpid(-1, &status, 0);
```

The `waitpid()` system call suspends execution of the calling process until a child specified by the _pid_ argument has changed state.
  - by default, `waitpid()` waits only for terminated children, but this behavior is modifiable via the _options_ argument


**PID values**

Value of _pid_ can be:
  - < -1 : wait for any child process whose process group IS is equal to abs(pid)
  - -1 : wait for any child process
  - 0 : wait for any child process whose process group ID is equal to that of the calling process
  - > 1 : wait for any process whose process ID is equal to the value of _pid_


### Options
- WNOHANG: return immediately if no child has exited
- WUNTRACED: also return if a child has stopped
- WCONTINUED: also return if a stopped child has been resumed by delivery of SIGCONT

### Return values
On success:
  - returns process ID of terminated child

On failure:
  - returns -1

### Note
If you call `wait()` and the child process has already returned, then `wait()` gets the return value of the terminated child immediately.


## `kill()`


```c
int kill(pid_t pid, int sig);
```

The `kill()` system call can be used to send any signal to any process group of process.

### Return values
On success (at least one signal was sent)
  - 0 is returned

On failure
  - -1 is returned

### List of signals
- SIGHUP : hangup
- SIGINT : interrupt from keyboard
- SIGKILL : kill signal
- SIGTERM : termination signal (least dangerous)
- SIGSTOP : stop the process
- SIGCONT : resume process

## `exec*()` family

```c
int execl(const char *path, const char *arg, ...);
int execlp(const char *file, const char *arg, ...);
int execle(const char *path, const char *arg, ..., char *const envp[]);

int execv(const char *path, char *const argv[]);
int execvp(const char *file, char *const argv[]);
int execvpe(const char *file, char *const argv[], char *const envp[]);
```

The `exec()` family of functions replaces the current process image with a new process image.

`exec*()` tells the OS kernel to replace the current program with a new program. The kernel roughly:
  1. Saves a copy of the new argv array and a copy of the environment
  2. Frees any private memory area
  3. Disconnects the process from any shared memory including a shared text segment and any shared libraries
  
At this point, any code following the `exec()` call no longer exists in the process. The same is true for any code prior to the `exec()` call and even the `exec()` call itself. It's all gone!

  4. Reads in the exec'ed program and reconstructs a working program. The environment and the arguments are copied into the new program
  5. Inherits the attributes from the calling process (ie. PID, PPID, NICE, etc...)
  6. Transfers control to the newly created program




### Return values
The `exec()` functions only return if an error has occurred. The return value is -1.
