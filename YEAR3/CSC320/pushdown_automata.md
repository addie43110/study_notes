# Pushdown Automata
  - An NFA with the addition of a stack
  - the stack provides additional memory
  - languages recognized by pushdown automata are exactly the context-free languages

## Formal definition
A pushdown automaton (PDA) is a 6-tuple (_Q, E, T, d, q0, F_) with:
  - _Q_: finite set of states
  - E: finite input alphabet
  - T: finite stack alphabet
  - d: the transition function
  - q0: start state
  - F: set of accept states

# Theorem
A language is context free if and only if some PDA recognizes it.

## Proof idea
Since every context free language L can be produced by context free grammar G, L = L(G), convert G into a PDA M. (Given a context-free grammar, create a PDA)

Alternatively, given a PDA, create a context-free grammar. 

## 1. Given a grammar, create PDA
  1. Place marker symbol ($) on stack
  2. Place start variable (S) on stack
  3. For each top stack symbol: replace it with it's derivation. ie. if variable _A_, then choose some rule _A_ -> _a1, a2, a3, ... , ak_ and substitute _A_ with _a1_ as the new top symbol.
  4. If terminal _a_, read next input symbol _w_ and pop if _w_ == _a_
  5. if $ go to accept state


Need at least three states: q_start, q_loop and q_accept. While reading symbols and replacing . popping, stay in q_loop. Only move to q_accept if we read $.

## 2. Given PDA, create grammar
  1. Simplify PDA
    - single accept state
    - $ always popped exactly before moving into accept state
    - transition: either push or pop, not both at the same time
  2. Design grammar
    - for any two states _p, q_ in _Q_, let L_p_q be the language that is recognized when starting at p with an empty stack and ending at q with an empty stack
  ... TODO: come back to this one.


