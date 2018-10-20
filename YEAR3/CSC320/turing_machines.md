# Turing machines

## What does a Turing machine look like?
  - has infinite tape representing unlimited memory
  - has tape head that can 
    - read symbols
    - write symbols
    - move around on the tape
  - intially, tape contains only input string, blank everywhere else
  - outputs 'accept' and 'reject' are obtained by entering designated accept and reject states
  - if it does not accept or reject, the computation will go on forever (infinite loop)

# Differences between finite automata and Turing machines
  1. Turing machine can both write on and read from tape
  2. Read-write head can move both left and right
  3. Tape is infinite
  4. Special states for rejecting and accepting take effect immediately (no need to finish reading the input)


# Turing machine formal definition
A turing machine is a 7-tuple (_Q, E, T, d, q0, q_accept, q_reject) where
  - _Q, E, T_ are finite sets
  - Q: set of states
  - E: input alphabet not containing blank symbol
  - T: tape alphabet, including blank symbol and all input symbols
  - d: transition function
  - q0: start state


# Configuration
A configuration is a "snapshot" of the TM in time. It consists of:
  - current state
  - current tape cntents
  - and current head location


Special cases: 
  - left-hand end:
    - we prevent machine from going off left-hand end of the tape
    - if we want to go left and write something, we simply end up overwriting what was at the start of the tape
  - right-hand end:
    - we assume that blanks follow whatever is at the end of our string / input / output
  - start configuration _q0w_
    - input is _w_ and machine in state _q0_
  - halting configurations
    - state is q_accept or q_reject
    - known as accepting and rejecting configurations

# Possible outcomes of TM computation
  - accept
  - reject
  - loop - machine fails to accept

A TM that halts on every input is called a decider.

# Turing machines & languages
  - if a language is recognized by some TM, then it is Turing-recognizable
  - if a language is Turing-decidable, there must be a decider TM that recognizes it
  - note: every Turing-decidable language is also Turing-recognizable

# More variants of Turing machines
  - multitape turing machines
  - nondeterministic Turing machines
  - enumerators

# Multitape TM
  - exactly like regular Turing machine but can Read, Write and Stop on any one or all of their tapes for each computation
  - all tapes start off blank, except for first tape which has the input string

# Theorem
Every multitape TM can be written as a single tape TM that recognizes the same language

Proof
  - can just add new symbol # to denote the start of a new tape

# Nondeterministic TMs
  - all TMs are so far dterministic as their computation is deterministic due to the definition of the transition function
  - a nondeterministic TM is definied just like the single-tape TM but
  - when we are in some state and we read a symbol, we can now go to multiple other states instead of just one.

# Theorem
Every nondeterministic TM has an equivalent (deterministic TM).

Proof: simulate a nondeterministic TM N with deterministic multi-tape TM D.

Idea: D executes computation of every branch in computation tree of N in BFS-manner, unless it enters its accept state
