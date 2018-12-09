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

The `exec()` family of functions replaces the current process image with a new process image.
```

### Return values
The `exec()` functions only return if an error has occurred. The return value is -1.
