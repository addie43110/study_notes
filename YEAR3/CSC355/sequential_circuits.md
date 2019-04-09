# SEQUENTIAL CIRCUITS

![sequential](sequential.png)

## Synchronous and Asynchronous
There are two types of sequential circuits
- synchronous:
  - changes in output at discrete instants of time
  - a clock synchronizes all circuit elements
- asynchronous
  - behaviour directly controlled by input changes
  - no clock
    - no wait for clock pulses (potentially faster)

## S-R Latch
- single bit storage device
- S = set
- R = reset
- undefined when both S and R are 1
![sr latch](sr_latch.png)

State Table

S(t) | R(t) | Q(t) | Q+
---|---|---|---
0 | 0 | 0 | 0
0 | 0 | 1 | 1
0 | 1 | 0 | 0
0 | 1 | 1 | 0
1 | 0 | 0 | 1
1 | 0 | 1 | 1
1 | 1 | 0 | X
1 | 1 | 1 | X

![sr-latchdiag](sr-latchdiag.png)

### Developing a characteristic equation
Put the state table on a K-Map to determine the Q+ (characteristic) equation. Here, Q+ = S + !RQ, SR=0

### Clocked S-R Latch (S-R Flip Flop)
![srff](srff.png)

Inputs must wait until clock is 1 before they can be passed onto the rest of the circuit. When clock is 0, S and R are both 0 (state of the circuit is held)

If you make the SR latch with NAND gates instead of NOR gates, it changes it to an active low (low control) circuit. Thus, S and R are active when they are 0 instead of 1. Undefined behaviour when S and R are both 0.
