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

# Signal vs broadcast
```c
pthread_cond_signal(&myConvar)
```
will only wake up one thread.

```c
pthread_cond_broadcast(&myConvar)
```
will wake up all threads.

# Lose wakeup signal
  - when there is no thread waiting for a convar and the wakeup signal is sent by `pthread_cond_signal(&convar) will disappear

# Assignment 2 ideas
Ideas:
  - you should have a thread for each clerk
  - you should create a thread for each customer as well
  - create all these threads at one time
    - total number of threads wuold be 4+n_cust+1
  - there might be 6 condition variables
    - each clerk has its own condition variables and each queue has a condition variable as well

More ideas:
  - create the customer threads and then let them sleep until they reach arrival time
  - either the clerk is idle or busy; every time the clerk is idle we can send them
    - when the clerk is idle, we send a signal to a queue
    - the queue wakes up the customers and then decides which customer is at the head of the queue, then sends a signal back to the clerk
  - this seems like a really complicated way of doing it


# Next week
Next week, we might get a file with Huang's ideas.
