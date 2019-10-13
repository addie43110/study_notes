# Rule-Based Classifiers

The goal:
- classify records by using a collection of "if...then..." rules
- ex. (blood_type = warm) ^ (lay_eggs = yes) -> bird
- a rule is of form (condition) -> y
  - condition is a conjunction of attributes
  - y is the class label
  - LHS: rule antecedent (condition)
  - RHS: rule consequent

## Application of rule-based classifier
- a rule _r_ covers an instance __x__ if the attributes of the instance satisfy the condition of the rule

### Rule coverage and accuracy
- coverage
  - fraction of records that satisfy the antecedent of a rule
- accuracy
  - fraction of records that satisfy both the antecedent and consequent of a rule (over those that satisfy the antecedent)

## Decision trees vs. rules
Converting from trees to rules is easy.
- one rule for each leaf
- antecedent contains a condition for every node on the path from the root to the leaf
- consequent is the class assigned by the leaf
- straightforward, but rule set might be overly complex

Converting a set of rules into a tree is more difficult
- a tree cannot easily express disjunction between rules
- example:
  - if a and b, then x
  - if c and d, then x
- the corresponding tree contains identical subtrees ("replicated subtree problem")

## Desiderata for rule-based classifier
- ensure that every record is covered by at most one rule
  - order rules based on their quality
- ensire that every record is covered by at least one rule
  - use a default class
- when a new record is presented to the lassifier
  - it is assigned to the class of the highest ranked rule it triggers
  - if none of the rules is fired, it is assigned to the default class

## A simple covering algorithm
- generate a rule by adding tests that maximize that rule's accuracy
- each new test (growing the rule) reduces the rule's coverage, but it increases the rule's accuracy

### Selecting a test
- the goal is to mximize accuracy
  - t: total number of instances covered by rule
  - p: positive (correct) examples of the class covered by rule
  - select test that maximizes the accuracy of p/t
- we are finished when p/t = 1 or the set of instances can't be split any further

### Pseudo-code for PRISM
```
for each class C
  initialize E to the instance set
  while (E constains insances of class C)
    create a rule R with an empty left-hand side that predicts class C
    until R is perfect (or there are no more attributes to use) do
      for each attribute A not mentioned in R, and each value v
        consider adding the condition A=v to the left-hand side of R
        select A and v to maximize the accuracy of p/t
        (break ties by choosing the condition with the greatest t)
      add A=v to R
    remove the instances covered by R from E
```

### Miscellaneous notes
RIPPER algorithm is similary to PRISM, except that it uses information gain instead of p/t. Also it uses tests:
  - x = value (for discrete attributes)
  - x <= value (for numerics)
  - x > value (for numerics)
