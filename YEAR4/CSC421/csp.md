# Constraint satisfaction problems (CSPs)
- a constraint satisfaction problem (or CSP) is defined by:
  - a set of variables: X1, X2, ... , Xn
  - a set of domains: D1, D2, ... , Dn
  - a set of constraints: C1, C2, ... , Cm
- an assignment that does not violate anz constraints is called a consistent or legal assignment
- complete assignment is one in which every variable is mentioned
- a solution to a CSP is a complete assignment that satisfies all the constraints
- binary CSP: each constraint relates two variables
- constraint graph: nodes are variables, arcs are constraints

## Varieties of constraints
- unary constraints involve a single variable
  - eg. SA != green
  - can be dealt with by filtering the domain of involved variables
- binary constraints involve pairs of variables
  - eg. SA != WA
- higher-order constraints involve 3 or more variables

## CSP as a search problem
- state: is defined by an assignment of variables Xi with values from domain Di
- initial state: the empty assignment {}, in which all variables are unassigned
- successor function: a value can be assigned to any unassigned variables, provided that it does not conflict with previously assigned variables
- goal test: the current assignment is complete (all variables are assigned values complying to the set of constraints)
- path cost: constant (eg. 1) for every step
- every solution must be a complete assignment and therefore appears at depth n if there are n variables
- furthermore, the search tree extends only to depth n
- for these reasons, depth-first search algorithms are popular for CSP's
- it is also the case that the path by which a solution is reached is irrelevant

## Standard search formulation problem
- if we try the classical search on a CSP, we get a branching factor at top level b = n * d, because any of the d values can be assigned to any of n variables.
- at the next level, the branching factor is (n-1)\*d (in the worst case, eg. when there are no constaints at all)
- we generate a tree with n!\*d^n leaves although there are only d^n possible assignments

## Backtracking search
- variable assignments are commutative (ie. WA = red then NT = green is the same as NT = green then WA = red)
- so, we only need to consider assignments to a single variable at each search node
  - b = d and there are d^n leaves generated
  - again, this is in the worst class, when there are no constraints at all
- depth first search for CSP's with single-variable assignments is called backtracking search
  - backtracking search is the basic algorithm for CSP's
- recall, we are looking for ANY solution
  - all solutions have the same depth and hence the same cost
  - now, by picking a good order to try the variables, we can drastically cut down our search

### A closer look at `expand(assignment, csp)`
- to expand an assignment means to assign a value at an unassigned variable which complies to the constraints
- which unassigned variable we pick for assignment turns out to be very important in quickly finding a solution
- in other words, the order of the list of successors returned by `expand(...)` is very important in quickly finding a solution

### Most constrained variable (MRV)
- choose the variable with the fewest legal values
- minimum remaining values (MRV) heuristic

### Degree heuristic (D)
- the MRV heuristic doesn't initially help if every variable has the same number of legal options
- in this case, the degree heuristic comes in handy
  - it attemps to reduce the branching factor on futue choices by selecting the variable that is involved in the largest number of constraints on other unassigned variables
- not only the first time: the MRV heuristic is usually a more powerful guide, but the degree heuristic can be useful as a tie-breaker

### Forward checking (FC)
- idea:
  - keep track of remaining legal values for unassigned variables
  - terminate search when any variable has no legal values
