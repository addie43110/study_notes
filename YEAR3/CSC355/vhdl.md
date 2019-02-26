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

### Port declaration
```vhdl
PORT (in1 :     IN integer;
      in2 :     IN bit_vector(2 downto 0);
      in3 :     IN std_logic;
      out1:     OUT bit;
      out_bus:  INOUT bit_vector(2 downto 0)
);
```

- port name, eg. `in1`, is composed of letters, numbers, and undercores
- mode is the direction of the signal, `IN`, `OUT`, and `INOUT`
- and type is one of the possible types

### Entity example: car light

```vhdl
ENTITY car_light IS
  GENERIC (length : integer := 4); -- a constant declaration
  PORT (doors : IN bit_VECTOR (length-1 downto 0);
        internal_switch : IN bit;
        light : OUT bit);
END ENTITY car_light;
```


### Example: 4-1 Multiplexer (8-bit inputs)

```vhdl
ENTITY mux IS
  PORT (I0, I1: IN bit_vector(7 downto 0);
        I2, I3: IN bit_vector(7 downto 0);
        Sel   : IN bit_vector(1 downto 0);
        z     : OUT bit_vector(7 downto 0)
);
END ENTITY mux;
```

### Three types of architecture bodies
- concurrent statements
- processes
- structural components

```vhdl
ARCHITECTURE <architecture name> OF <entity name> IS

  <declarations> -- signal or component declarations

BEGIN
  out1 <= in1 AND b; -- dataflow

  PROCESS (IN1, B) -- process
  BEGIN
    OUT1 <= IN1 and B;
  END PROCESS;

  Inst1: halfadder -- component instantiation
  PORT MAP (a=> in1, b=> in2, out1=> sum, out2 => carry);  
END <architecture name>;
```

### Architecture of a halfadder

```vhdl
LIBRARY IEEE;
USE IEEE.std_logic_1164.all;

ENTITY hfadder IS
  PORT (X, Y: IN std_ulogic;
        Sum, Carry: OUT std_ulogic);
END ENTITY hfadder;

ARCHITECTURE dataflow OF hfadder IS

BEGIN
  Sum <= (X XOR Y) AFTER 5 NS; -- add timing
  Carry <= (X AND Y) AFTER 3 NS;
END ARCHITECTURE dataflow;
```

`std_logic` resolved to `std_ulogic`, which has 'U', 'X', '0', '1', 'Z', 'W', 'L', 'H', and '-' types.

### Signals
- a repository of a time value (waveform)
- receives a value at a specific point in time and retains it until a new value is assigned at a future point in time
- ports are signals
  - with associated direction (IN, OUT)
- signals delared inside an architecture have no mode associated with them
  - they are only visible inside the architecture

```vhdl
DEFINITION
  SIGNAL <signal name> : <type> ;
```

### Full adder
```vhdl
ARCHITECTURE structural OF full_adder

  COMPONENT half_adder
    PORT(a, b: IN std_logic;
        sum, carry: OUT std_logic);
  END COMPONENT half_adder;

  COMPONENT or_2
    PORT(a, b: IN std_logic;
         c   : OUT std_logic);
  END COMPONENT or_2;
  
  SIGNAL s1, s2, s3 : std_logic;

BEGIN
  H1: half_adder PORT MAP
      (a=>in1, b=>in2, sum=>s1, carry=>s3);
  H2: half_adder PORT MAP
      (a=>s1, b=>cin, sum=>Ssum, carry=>s2);
  M1: or_2 PORT MAP (a=>s2, b=>s3, c=>cout);
END structural;
```

## Processes
- can be viewed as a replacement for a concurrent assignment statement that permits more complex descriptions
- uses procedural assignment statements similar to a typical imperative programming language
- multiple processes can execute concurrently
- processes are concurrently activated but the statements inside the body are executed **sequentially**

### Control flow constructs
- if... then... else... end if;
- for .. end loop;
- case ... is ... end case;
- a <= "01"; (signal assignment)
- b := "01"; (variable assignment)


CASE
```vhdl
ARCHITECTURE p1 OF half_adder IS
BEGIN
sum_proc: PROCESS (a, b)
  BEGIN
    IF (a = b) THEN
      sum <= '0' AFTER 5 NS;
    ELSE
      sum <= (a OR b) AFTER 5 NS;
    END IF;
  END PROCESS sum_proc;

carry_proc: PROCESS(a, b)
  BEGIN
    CASE a IS
      WHEN '0' =>
        carry <= a;
      WHEN '1' =>
        carry <= b;
      WHEN OTHERS =>
        carry <= 'X';
    END CASE;
  END PROCESS carry_proc;
END ARCHITECTURE p1;
```

SELECT
```vhdl
BEGIN
  with Z select
    CS <= "00" when "000",
          "01" when "001",
            ...
```


WHEN ELSE
```vhdl
BEGIN
  A <=      "100" when D(4) = '1'
       else "011" when D(4 downto 3) = "01"
       else "010" when D(4 downto 2) = "001"
       else "001" when D(4 downto 1) = "0001"
       else "000" when D = "00001"
       else "XXX";
  V <= not (D = "00000");
```


### Process behaviour
- some processes never teminate; infinite loops
- can `wait on` a signal
- a process waits for a signal in the sensitivity list to change
  - `process(sig1, sig2, sig3)`
- once the process executes, any updates to the signals happens ONLY at the end of the process
  - if a signal is updated that is in the sensitivity list, the process restarts automatically
- variables are updated right away

```vhdl
PROCESS (C,D)
  VARIABLE G,H : integer := 0;
BEGIN
  A <= 2;
  B <= A + C;
  G := A + 3;
  A <= D + 1;
  H := D + 2;
  E <= A * 2;
  F <= G + H;
END PROCESS;
```
