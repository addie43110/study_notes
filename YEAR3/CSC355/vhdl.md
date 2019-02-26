# VHDL

VHSIC HDL = Very High Speed Integrated Circuit Hardware Description Language

There are five types of primitive constructs
1. Entity declaration
2. Architecture body
3. Configuration declaration
4. Package declaration
5. Package body

## Packages
- collection of definitions of data types, subprograms, and constants
- each package belongs to a library
- libraries refer to a groups of VHDL objects
- packages must be declared before describing an entity

To use a package
1. declare the library (`LIBRARY`)
2. indicate what you want to use from the package (`USE`)

```vhdl
LIBRARY IEEE;
USE IEEE.std_logic_1164.all;
```

## Entities and Architextures
Entity
- primary hardware abstraction
- provides: entity name, inputs and outputs
- analogous to a symbl in a block diagram

Architecture
- specifies relationships between inputs and outputs
- may be a mixture of structural, concurrent and sequential VHDL
- a given entity may have multiple, different architectures

### Entity declarations
```vhdl
ENTITY hfadder IS
  PORT (X, Y: IN BIT;
        Sum, Carry: OUT BIT);
END ENTITY hfadder;
```
