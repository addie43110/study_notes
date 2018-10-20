# Context-free grammars
Formal definition.

A context-free grammar is a 4-tuple $`(V, Σ, R, S)`$ where
  - $`V`$ is a finite set of variables
  - $`Σ`$ is a finite set of terminals (disjoint from _V_)
  - _R_ are the rules
  - and _S_ is the start variable

# Leftmost derivations
We call a derivation of string _w_ in grammar _G_ leftmost derivation if at every step the leftmost remaining variable is replaced.

# Ambiguous grammars
  - a string is derived ambiguously if it has at least two different leftmost derivations
  - such a grammar is called ambiguous

# Chomsky Normal Form
  - a restricted / simplified constraints on grammar
  - must follow where every rule is of the form:
    - A -> BC or A -> a where
      - a is a terminal
      - A, B, and C are variables
      - B and C cannot be the start variable
      - and only the start variable can go to epsilon

# Theorem
Any context-free language can be turned into a context-free grammar in Chomsky normal form.

Idea: We know that any context-free language can be written as a context-free grammar. Thus, given some context-free grammar, convert it into Chomsky normal form.

1. Add new start variable
2. Eliminate all ε-rules of form A -> ε
3. Eliminate all unit rules
4. Clean up remaining rules


# Designing Context-free Grammars (CFGs)
  - many context free languages are a union of simpler context free languages
  - merge individual grammars by combining their rules and adding one "merging" rule for each start variable
  - given two context-free languages, 

```math
L_1 union L_2 = L(G) with
G = (V_1 union V_2, epsilon, R_1 union R_2 union {S -> S_1 | S_2} S)
```
