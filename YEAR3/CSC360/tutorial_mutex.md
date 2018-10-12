# Mutex

Can initialize a mutex statically,
```
pthread_mutex_t name = PTHREAD_MUTEX_INITIALIZER
```

or dynamically
```
pthread_mutex_init(attr, ...)
```

# Mutex variable
`pthread_mutex_trylock(&myMutex);` will return immediately no matter if mutex is locked or not
  - two cases:
    1. When mutex is unlocked, return 0
    2. When mutex is locked by another thread already, return `EBUSY`

# Lock and unlock
If you use lock and unlock many times, it will make your program slow. We need to learn to use it efficiently.

## When should we use a mutex lock?
  - when we enter a critical section

# Condition variables
  - can initialize statically or dynamically as well
  - `pthread_cond_wait(pthread_cond_t *cond, pthread_muted_t *mutex);`

Example:
```c
pthread_mutex_lock(&mutex);
pthread_cond_wait(&convar, &mutex);
  ...
pthread_mutex_unlock(&mutex);
```
