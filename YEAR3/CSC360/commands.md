# Commands

# `wait`
```c
pid_t wait(int *status);
```
`wait()` suspends execution of the calling process until one of its children terminates.


## Return values
  - -1 if error
  - process ID of terminated child if successful

# `waitpid`
```c
pid_t waitpid(pid_t pid, int *status, int options);
```

`waitpid() ` suspends execution of the calling process until a child specified by pid argument has changed state. By default, `waitpid()` only waits for terminated children, but you can modify this behavious via the arguments.

## Return values
  - returns process ID of the child is successful
  - if WNOHANG specified, but no child has changed state, then 0 is returned
  - on error, -1 is returned

# `kill`
```c
int kill(pid_t pid, int sig);
```
If pid is:
  - positive, signal is sent to the process with that pid
  - 0, signal is sent to every process in the process group of the calling process

List of signals:
  - `SIGTERM` - termination signal
  - `SIGCONT` - continue
  - `SIGSTOP` - stop process
  - `SIGKILL` - kill process but not safely

## Return values
  - 0 on success
  - -1 if error

# `exec` family

```c
int execl (const char *path, const char *arg, ...);
int execlp(const char *file, const char *arg, ...);
int execle(const char *path, const char *arg, ... , char * const envp[]);
int execv(const char *path, char *const argv[]);
int execvp(const char *file, char *const argv[]);
int execvpe(const char *file, char *const argv[], char *const envp[]);
```

`execl()`, `execlp()`, and `execle()` all take separate arguments as the ...
<br/>
`execv()`, `execvp()`, and `execvpe()` take only one argv array with all the arguments.
<br/>
The `exec()` family of functions replaces the current process image with a new process image.

## Return values
The `exec()` functions return only if an error has occurred. Return value is -1.


# `pthread_create()`
```c
int pthread_create(pthread_t *thread, const pthread_attr_t *attr, void *(*start_routine)(void*), void *arg);
```

`pthread_create()` creates a new thread, with attributes specified by `attr`, within a process. If `attr` is `NULL`, the default attributes are used. `thread` is just a `pthread_t` where `pthread_create()` can store the ID of the thread.


## Return values
  - 0 if successful
  - error number if error

# `pthread_join()`
```c
int pthread_join(pthread_t thread, void **value_ptr);
```

`pthread_join()` suspends execution of the calling thread until the target thread terminates, unless the target thread has already terminated. Here, `thread` is the actual value and not the pointer! The value given to the `pthread_exit(#)` will be made available to the `value_ptr` argument.

## Return values
   - 0 if successful
   - error number if error 
