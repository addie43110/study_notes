# Serialization
- unfortunately, not much was discussed on this topic

# Motivation
- we often want to "persist" data
  - save the state of our program and then read it in the next time the program starts up
- we can do this a number of ways
  - writing to an XML file
  - saving in a .txt file or .csv file
  - etc.
- we can also use Java's `serializable` interface
  - but we probably shouldn't
  - it is ridden with issues
  - so just know about it for the exam

## Java Serialization
- implement the `serializable` interface

To write an object out...
```java
class Employee implements Serializable {

  public static void main(String[] args) {
    ObjectOutputStream out = new ObjectOutputStream(new FileOutputStream("..."));
    Employee bob = new Employee("Bob");
    out.write(bob);
    ...
  }
}
```

To read an object in...
```java
class Employee implements Serializable {
  
  public static void main(String[] args) {
    ObjectInputStream in = new ObjectInputStream(new FileInputStream("..."));
    Employee bob = in.readObject();
  }
}
```

## Issues
- security
  - still could read in some malicious object
- performance problems
  - do we need to serialize everything?
  - perhaps this makes serialization "too easy", thus leading to lazy programming
- maintainability
  - the class definition is rigid now
  - can't read in a different object
  - plus, the file written out is not human-readable
    - which makes it difficult to check for problems

An alternative? Nernst suggests using JSON.

# Object Relational Mapping (ORM)
- perhaps we want to map from some object diagram to a relationship diagram
- this is possible, but may introduce a "leaky abstraction"
  - certain aspects of the object diagram or relationship diagram are ignored
  - for example: to get an `Employee`'s `Salary`, we can use (perhaps) `Employee.getSalary()`
  - but translating this to a database may require a join such as `join employee.id = salary.id`
  - since joins are expensive (can take a long time), this may not be the best way to implement in a database

# Refactoring
- changing code, but not the functionality
- just making it "cleaner"
  - clearer
  - easier to read
  - easier to maintain
  - improve design
  - improve quality
- important not to break anything while refactoring
  - good idea to (continue to) run tests
- because there is a high chance you will introduce a bug, important to focus only on code that will likely change instead of code which does not need to be touched again

## Examples of refactoring
- remove duplicate code
- rename variables and methods
- clean comments or add comments

## Code smells
- sometimes the code just "smells"
- I personally hate that term
- so I will use "seems fishy"
- sometimes the code just "seems fishy"

### Shotgun surgery
- if you change one thing in the code, you have to change a bunch of things in multiple places
- this represents high coupling
- maybe time to refactor the code?

### God methods / classes
- long methods or classes that seem to do a lot of stuff
- violates Single Responsibility Principle
- could break up into multiple methods / classes to make code clearer and easier to maintain
- alternatively, if this code is initialization code that will never be changed, it would be better to just leave it
  - no need to refactor code that doesn't need to be touched

### Long parameter lists
- a method takes in many many parameters
- perhaps put these parameters in an object and just pass the one object as the parameter

### Primitive obsession
- dirty `C` programmers may have "primitive obsession"
- obsession with maximizing program executable efficiency
- force poor abstractings to be primitives with the idea that the code will run faster
  - but this makes the code difficult to read and maintain
- ex. forcing a telephone number to be an `int`
  - much more difficult to get area code of the telephone number
  - have to do bitwise operations
