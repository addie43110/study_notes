# Logical Agents

## Example: Wumpus world
- Wumpus world is a cae consisting of rooms connected by passageways
- lurking somewhere in the cave is the terrible wumpus, a beast that eats anyone who enters its room
- the wumpus can be shot by an agent, but the agent only has one arrow
- some rooms contain bottomless pits that will trap anyone who wanders into these rooms (except for the wumpus, which is too big to fall in)
- the only mitigating feature of this bleak environment is the possibility of finding a heap of gold

### Game rules
- performance measures:
  - gold +1000
  - death -1000
  - -1 per step
  - -10 for using the arrow
- environment
  - squares adjacent to wumpus are smelly
  - squares adjacent to a pit are breezy
  - glitter iff gold is in the same square
  - shooting kills wumpus if you are facing it
  - shooting uses up the only arrow
  - grabbing picks up gold if in same square
- actuators
  - turn left
  - turn right
  - forward
  - grab
  - release
  - shoot
- sensors
  - breeze
  - glitter
  - smell

## Logic in general
- logics are formal languages for representing information
  - such that conclusions can be drawn
- syntax defines the sentences
  - allowed in the language
- semantics define the "meaning" of sentences
  - ie. define the truth of sentences in a world

## Proposition logic
- examples:
  - raining
  - r32aining
  - rAiNiNg
  - raining\_or\_snowing
- non-examples:
  - 324567
  - raining.or.snowing

## Compound sentences
- negations
  - -raining
- conjunctions
  - raining ^ snowing
- disjunctions
  - raining v snowing
- implications
  - raining -> cloudy
    - the left argument of an implication is the antecedent
    - the right argument of an implication is the consequent
- reductions
  - cloudy <- raining
    - the left argument is the consequent
    - the right argument is the antecedent
- equivalences
  - cloudy <-> raining

## Operator precedence
- not, and, or, implications

## Interpretation
- an interpretation (i) is an association between the propositional constants and truth values T or F
- for example:
  - p = T
  - q - F
  - r = T

## Operator semantics
- from a given interpretation we can derive the truth of any sentence based on the recursive application of the following rules
- pretty much just a truth table

## Evaluation
- consider the interpretation i shown below:
  - pi = T
  - qi = F
  - ri = T
- we can see that i satisfies (p v q) ^ (-q v r)
  - (because it evaluates to true)
- now consider the following interpretation j:
  - pi = T
  - qi = T
  - ri = F
- in this case, j does not satisfy (p v q) ^ (-q v r)
  - because it evaluates to false

## Satisfaction and models
- if based on a given interpretation a sentence is determined to be true (we say "satisfied"), the interpretation is called a model of that sentence
- we can also say "the sentence is true in that model"

## Models and entailment
- let a be a sentence
- we say m is a model of a sentence a if a is true in m
- M(a) is the set of all models of a
- a entails B, denoted by a|=B if and only if M(a) is a subset of M(B)
- eg. sentence p entails sentence p v q
  - since a disjunction is true whenever (in every model that) one of its disjuncts is true, then p v q must be true whenever p is true
- on the other hand, sentence p does not logically entail p ^ q
  - a conjunction is true if and only if both of its conjuncts are true, and q may be false

## Knowledge bases
- a KB (knowledge base) is a set of sentences
- we say m is a model of KB if all sentences of KB are true in m

## Validity, satisfiability, and unsatisfiability
- a sentence of valid if and only if it is satisfied by every interpretation
  - p v -p is valid
- a sentence is satisfiable if and only if it is satisfied by at least one interpretation
  - eg. most typical sentences
- a sentence is unsatisfiable if and only if it is not satisfied by any interpretation
  - p <->  -p is unsatisfiable
  - no matter what interpretation we take, the sentence is always false

## Semantic reasoning methods
- several basic methods for determining whether a given set of premises propositionally entails a given conclusion

## Truth table method
- one way of determining whether or not a set of sentences (premises) entail a possible conclusion (another sentence) is to check the truth table
- method (build two truth tables)
  - start with a complete truth table for the propositional constants
- iterate through all the premises 
  - for each premise eliminate any row that does not satisfy the premise
- build a similar table for the conclusion sentence
- finally, compare the two tables
  - if every row that remains in the premise table also remains in the conclusion table, then the premises logically entail the conclusion

## Validity checking
- single table approach
  - to determine whether a set of sentences {p1,...,pn} logically entails a sentence p, we form the sentence (p1 ^ .. ^ pn -> p) and check if that is valid
- then form a truth table with an added column for this sentence and check its satisfaction under each of the possible interpretations for our logical constants

## Unsatisfiability checking
- another single table approach
- works negatively instead of positively
  - to determine whether a finite set of sentences {p1,...,pn} entails a sentence p, we form the sentence (p1 ^ ... ^ pn ^ -p) and check if that is unsatisfiable
- both validity checking and satisfiability checking require about the same amount of work as the truth table method, but they have the merit of manipulating only one table

## Rules of inference
- a rule of inference is a pattern of reasoning consisting of
  - a first set of sentence variables, called premises, and
  - a second set of sentence variables, called conclusions
- a rule of inference is sound iff for every instantiation of sentence variables, premises logically entail conclusions

### Example: modus ponens
```
p -> w
p
------
w
```

### Resolution
- using resolution, it is possible to build a theorem prover that is sound and complete for all of Propositional Logic
- soundness theorem: if p is provable using resolution from d, then d logically entails p
- completeness theorem: if d logically entails p, then p is provable using resolution from d

#### Clausal form
- resolution works only on expressions in clausal form
- some definitions
  - a literal is either an atomic sentence or a negation of an atomic sentence
    - eg. p, -p
  - a clause expression is either a literal or a disjunction of literals
    - eg. p, -p, p v q
  - a clause is the set of literals in a clause expression
    - eg. {p}, {-p}, {p, q}
    - note that {} is also a clause equivalent to the empty disjunction

#### Converting to clausal form (INDO)
1. Implications
  - p -> q  === -p v q
  - p <-> q === (-p v q) ^ (p v -q)
2. Negations
  - distribute negations using de Morgan's law, simplify
3. Distribution
  - a v (b ^ c)  === (a v b) ^ (a v c)
  - (a ^ b) v c === (a v c) ^ (b v c)
4. Operators
  - a v ... v z === {a,...,z}
  - a ^ ... ^ z === {a}...{z}

### Propositional resolution
- the idea of resolution is simple
- if p is false, then by the first clause, q must be true
- if p is tru, then, by the second clause, r must be true
- since p must be either true or false, then it must be the case that q is true or r is true
- in other words, we can cancel the p literals

```
{p,q}
{-p,r}
-------
{q,r}
```

Process:
- negate the goal
- add it to our premises
- use resolution to determine whether the resulting set is unsatisfiable (ie. the empty clause)

#### Two finger method (TFM)
- we keep two pointers (slow and fast)
- on each step we compare the sentences under the pointers. if we can resolve, we add the new derived sentence at the end of the list
- at the start of the inference, we initialize slow and fast at the top of the list
- as long as the two pointers point to different positions, we leave the slow where it is and advance the fast
- when they meet, we move the fast at the top of the list and we move the slow one positiong down the list

#### Tautology elimination
- a tautology is a clause with a complementary pair of literals

Example:
1. {p, q}     premise
2. {p, -p}    premise
3. {p, q}     1, 2 (not a new thing, we already knew it)

We can remove tautologies, such as {p, -p} without causing any harm.
