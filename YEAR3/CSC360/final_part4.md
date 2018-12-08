# Synchronization

## The need for synchronization
Consider the following code if it was run at the same time by multiple threads:

```C
int i;
for(i=0; i<100; i++) {
  count++;
}
```

It seems simple enough, but the important thing is to remember that this one command is actually syntactic sugar for three:

```assembly
ld [count] [reg]
inc [reg]
st [reg] [count]
```

This leads to the **race condition**, which is when multiple threads are trying to access the same variable at the same time.

## Critical sections
  - the code section where accessing of shared data occurs
  - in our example, that would be the `count++`
  - only one thread executing in the critical section at any time (this is what we want)

**Warning**: use the critical section carefully and _with correct scope_ to avoid deadlock!

## Properties of a correct solution to the critical section problem
1. Mutual exclusion: only one thread can be in the critical section at a time
2. Making progress: if no thread is in the critical section, the one who wishes to enter should be able to
3. Bounded waiting time: waiting time should not be forever (no deadlock!)

## Solutions to the critical section problem

### First try
```C
/* process 0 */

while(1) {
  while(turn!=1); /* spin loop */
  /* critical section */
  turn = 1;
  /* remainder section */
}


/* process 1 */
while(1) {
  while(turn != 0); /* spin loop */
  /* critical section */
  turn = 0;
  /* remainder section */
}
```

This solution is too polite. What happens if process 1 is very slow while process 0 is very fast? It can be that process 0 is left waiting for process 1 even though process 1 is not in the critical section. This violates making progress.

### Second try
```c
while(1) {
  flag[i] = 1;
  while(flag[j]); /* spin loop */
  /* critical section */
  flag[i] = 0;
  /* remainder section */
}
```

This solution is too aggressive. If both processes raise their "flags" at the same time, we instantly have deadlock.

### Peterson's solution
```c
do {
  flag[i] = true; /* I want to go! */
  turn = j; /* I give the turn to you */
  while(flag[j] && turn==j); /* if it is your turn and you want to go, wait */
  /* critical section */ /* else, it is my turn! */
  flag[i] = false; /* I am done, I lower my flag */
  /* remainder section */
} while (1);
```

Idea: I want to enter my critical section but I don't want to hold you up so I will set the turn over to you and wait until you tell me you are not interested in entering the critical section, or until it is my turn (whichever comes first). Of course, I expect you to do the same for me.

## Mutex
Use
```c
pthread_mutex_lock(&mutex);
/* critical section */
pthread_mutex_unlock(&mutex);
```

Known as **mutex lock**. (Mutex is short for mutual exclusion). A process must acquire the mutex lock before entering a critical section; it releases the lock when it exits the critical section.

Has two states:
  - locked
  - unlocked


## Semaphores
Definition: Semaphoe S is an integer variable that, apart from initialization, is accessed only through two standard _atomic_ operations: `wait()` and `signal()`. (_Atomic_: Uninterruptable).

Sematic of `wait()` and `signal()` are as follows:

```c
/* s.wait() */
while (s <=0); /* spin loop */
s--;

/* s.signal() */
s++;
```

Warning: Never use semaphore without setting an inital value!

## Condition Variables
Why do we use condition variables?
  - to avoid spin loops
  - when we need to continually check if some condition is met
  - can use condition variable to achieve this without spin loop

How to use condition variables
  - always used in conjunction with a mutex lock
  
Example: thread A
```c
{
  /* do something up to the point where a certain condition must be met */
  pthread_mutex_lock(&mut);
  while (!condition) {
    pthread_cond_wait(&cond, &mut);
    /* when signalled, will wake up and automatically lock mut */
    /* do stuff */
    pthread_mutex_unlock(&mut);
  }
  /* remainder stuff */
}
```

### Warning message
```c
pthread_cond_wait(&cond, &m);
```
1. Before you call this function, m must be locked
2. It automatically releases the mutex m while it waits
3. After signal is received and thread is awakened, the mutex is automatically locked again
4. Therefore, you program needs to unlock the mutex when the thread finishes

### Other notes
1. `pthread_cond_signal(&cond);`: Wake up another thread that is waiting on the condition variable. If multiple are waiting on the condition variable, up to OS to decide which one gets run.
2. `pthread_cond_broadcast(&cond);`: Wake up ALL threads waiting on a condition variable.

- use a while loop instead of an if loop when waiting on a condition
  - this avoids having two threads entering the critical section at one time


## Monitor
Monitor is an abstract data type (ADT) which is essentially like this:

```c
{
  /* shared variables */
  F1()
  F2()
  /* condition variables: these are not the same as the condition
     variables in pthread, they are a special datatype defined in
     monitor */
  /* initialization code */
}
```

For some condition variable `x` in Monitor, it also supports `x.wait()` and `x.signal()`.

The Monitor structure ensures that only one process at a time is active within the monitor. Mutual exclusion is guaranteed by this monitor.

### How to implement monitor
1. For each monitor, a semaphore named _mutex_, which is initialized to 1, is provided. A process must execute `mutex.wait()` before entering the monitor and must execute `mutex.signal()` after leaving the monitor.
2. An integer variable `next_count` is also provided to count the number of processes suspended on 'next', where 'next' is another semaphore with initial value set to 0.
3. For each F<sub>i</sub>, it is replaced by:

```c
mutex.wait();
/* body of Fi */
if (next_count > 0) next.signal();
else mutex.signal() /* leaving monitor */
```

4. For each condition variable `x` defined in the Monitor, we need to implement `x.wait()` and `x.signal()`. For each _x_ we first introduce a semaphore `x_sem` and integer `x_count`, both initialized to 0.

```c
x.wait() {
  x_count++;
  if(next_count > 0) next.signal();
  else mutex.signal();

  x_sem.wait();
  x_count--;
}

x.signal() {
  if (x_count > 0) {
    next_count++;
    x_sem.signal();
    next.wait();
    next_count--;
  }
}
```
