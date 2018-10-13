# Part 8: Process synchronization
## 1. The need for synchronization
Example: producer-consumer. Remember the count++ and count-- are actually 3 steps in assembly language: load, increment, and store. Thus, if the CPU switches during any one of these tasks, we may get inconsistent output.

## 2. Race condition
Trying to access a variable at the same time

## 3. Critical section
  - code section accessing shared data
  - only one thread executing in the critical section at any time instance
In the above xample, count++ and count-- are critical sections. Warning: use critical section carefully (need the right scope) to avoid deadlock!

### How to solve the critical section problem
At the application level, we use: `pthread_mutex_lock(&mutex);`
Surround the critical section with
```c
pthread_mutex_lock(&mutex);
/* critical section */
...
pthread_mutex_unlock(mutex);
```
At the kernel level, we learn how the solution is implemented (`pthread_mutex`)

### Properties of a correct solution to the critical section problem
  1. Mutual exclusion: only one thread can enter the critical section at a time
  2. Making progress: if no thread is in the critical section, the one who wants to enter the critical section should be able to
  3. Bounded waiting time: waiting time should not be forever for threads who wish to enter the critical section

## 4. Possible solutions to the critical section problem
Algorithm 1
```c
process 1
int turn = 0;
do {
  while (turn != 1); //wait
    critical section
  turn = 2;
  remainder section
} while (1);

process 2
do {
  while (turn != 2); //wait
    critical section
  turn = 1;
  remainder section
} while (1);

```
This solution is too 'polite'. Each process first checks if it is its turn, and goes when it is. But if process 1 is fast and process 2 is slow, think about the following scenario.
<br/>
Process 2 goes first, gets out of the critical section and sets the turn to process 1. Then process 1 goes and finishes, but process 2 is still going through its remainder section. It is not in the critical section, but process 1 can't get in. Thus, this breaks the rule "making progress".
<br/><br/>
Algorithm 2
```c
boolean flag[2];
initially, flag[0] = flag[1] = false

process 1
do {
  flag[1] = true;
  while (flag[2]);
  critical section
  flag[1] = false;
  remainder section
} while (1);

process 2
do {
  flag[2] = true;
  while (flag[1]);
  critical section
  flag[2] = false;
  remainder section
} while(1);
```

What is wrong with this? Well imagine both processes raise their flags at the same time. Then we are instantly in deadlock. This solution is too aggressive. 

## Dekker's solution
Idea: "I want to enter my critical section, but I don't want to hold you up so I will set the turn over to you and wait until you tell me you are not interested in entering the critical section, or until it is my turn (whichever comes first). Of course, I expect you to do the same for me."
```c
while (true) {
  flag[i] = true;
  while (flag[j]) {
    if (turn == j) {
      flag[i] = false;
      while (turn == j); //wait
      flag[i] = true;
    }
  }
  /* critical section */
  turn = j;
  flag[i] = false;
}
```

## Peterson's solution
```c
Process 1
do {
  flag[1] = true; //I want to go!
  turn = 2; // I set the turn to you
  while(flag[2] and turn==2); //wait if it is your turn and you want to go
  /* critical section */ //it is my turn!
  flag[1] = false; //I am done, no longer want to go
  /* remainder section */
} while (1);

Process 2
do {
  flag[2] = true;
  turn = 1;
  while (flag[1] and turn==1); //wait; notice here we cannot have deadlock since turn can only be one integer. can't be both 1 and 2 at the same time.
  /* critical section */
  flag[2] = false;
  /* remainder section */
} while (1);
```

