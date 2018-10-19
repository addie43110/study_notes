# An Airline Check-in System Notes & Tips
by Huan Wang (huanwang@uvic.ca)

# Man Pages for pthread
```
sudo apt-get install manpages-posix-dev
sudo apt-get install glibc-doc

man pthread_create
```
Or you can just go to the Linux man pages online.

# Design
For the clerk thread, you can create them all at once. 
First, read all the information from the txt file, then create all the clerk threads first. Once you create them, they will wait and become idle (check if any customers come to the queue).
<br/>
Next, we need to create the customer threads. You can create them all at once, or you can gradually create them according to the sequence of arrival time.
<br/>
First way is easier, but the second way is more realistic.

# Join
The sequence of join doesn't matter. But all your threads need to have joined in order to terminate the program.

# Customer thread
Hardest part: most things to do. Basically
  - First: we get the customer information by arguments. Then I know the arrival time and service time.
  - Then I simulate the arrival time, letting the thread sleep for the arrival time given. note that 6 -> 0.6 seconds -> usleep(6*100 000);
  - then pick the queue to enter (this is determined by class)
    - lock(), enter queue, length ++, pthread_cond_wait()
  - when the customer is signaled by one of the four clerks
    1. pthread_cond_wait() returns
    2. check if "I am" in the head of the queue, if NOT, go back to pthread_cond_wait()
    3. pthread_mutex_unlock()

# Awoke
We need to know which clerk awoke me.
  - sleep for as long as the service time, then send a signal to the clerk to tell the clerk you have finished serviced. then the clerk can become idle again.
  - idea: before the clerk sends the idle signal, they write their id into a status variable. then the customer can check the status variable (global variable) to see which clerk has summoned him.
  - clerk needs to check if the queue is busy or idle.


# Trouble shooting
- if multiple clerks become idle at the same time, this can be avoided by checking the status of the queue and the length of the queue.

# Illegal values
When the customers.txt input file is incorrect. Additionally, if it is a character instead of a number. Or negative values.

# Time
use `gettimeofday();` to get the start time and the current time.
