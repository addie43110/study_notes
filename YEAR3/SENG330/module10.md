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
