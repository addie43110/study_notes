# Inheritance

## Learning Objectives
- [ ] inheritance
- [ ] class hierarchies
- [ ] common problems with inheritance
- [ ] Templace Method Design Pattern

## Review
- in general, polymorphism makes a design extensible and reusable
- we can see this using interfaces
- however, interfaces do not provide any _implementation_
  - but we may want to use a default implementation for a common method
  - for example, a `getName()` method for any Class implementing type `Person` should always return the field `aName`
  - so interfaces can still result in code duplication
  - we can avoid this duplication using inheritance!
- inheritance: define a class with respect to some superclass
  - avoid repeating declarations of class members
  - use `extends` keyword in Java
  - ex. `public class Manager extends Employee`
  - in UML, inheritance is shown with a solid line and a white triangle pointing to the superclass

Example:
```java
Employee alice = new Manager();
```

- note that `alice` is still a `Manager`; she doesn't lose any of the `Manager` specific fields.
- we can get them back by typecasting
  - ex. `Manager manager = (Manager) alice;`
- important to note the difference between **compile-time** type and **run-time** type
  - compile-time type is the type of variable in which an object is stored
    - this can be run-time type, or any supertype of the run-time type
  - run-time type is the one returned by `getClass()`
    - the type that is mention after the keyword `new`

Consider the following example:
```java
public static boolean isManager(Object pObject) {
  return pObject instanceof Manager;
}

public static void main(String[] args) {
  Employee alice = new Manager(); /* 1 */
  Manager manager = (Manager) alice; /* 2 */
  boolean isManager1 = isManager(alice);
  boolean isManager2 = isManager(manager);
}
```

- at `/* 1 */`, an object is created that is run-type type `Manager` and assigned to a variable of static type `Employee`


## Inheriting Fields

```java
class Employee {
  private String aName;
  private int aSalary;
  ...
}

class Manager extends Employee {
  private int aBonus;
  ...
}
```

- `Manager` inherits `aName` and `aSalary`
  - because `aName` and `aSalary` are private, they can only be used in `Employee` class (directly)
  - thus, `Manager` _cannot use these fields directly_
  - but `Manager` still inherits them!
  - can use keyword `protected` if we want `Manager` to be able to use the fields directly
  - else, supply a "getter" in the `Employee` class
- need to use `super` keyword to "pass" initialization data up the chain of class to the superclass constructor


```java
class Employee {
  private String aName = "Albert";
  private int aSalary = 0;
  
  public Employee(String pName, int pSalary) {
    aName = pName;
    aSalary = pSalary;
  }
  ...
}

class Manager extends Employee {
  private int aBonus = 0;

  public Manager(String pName, int pSalary, int pBonus) {
    super(pName, pSalary);
    aBonus = pBonus;
  }
  ...
}
```

## Inheriting methods
- methods are inherited similarly to how fields are inherited
- but what if we inherit a method that doesn't do what we want?
  - ex. `Employee` has method `getCompensation()` that returns `aSalary`
  - `Manager` extends `Employee` but when `getCompensation()` is called, we want to return `aSalary + aBonus`
- to fix this, we can _override_ methods
- but then which method do we choose in the following case:

```java
Employee employee = new Manager(...);
int compensation = employee.getCompensation();
```

- we choose the best fitting _run-time_ type method
  - in this case, run-time type of `employee` is `Manager`, so we select the `Manager` `getCompensation()` method
  - note that we must replace the `Manager` `getCompensation()` method with

```java
public int getCompensation() {
  return super.getCompensation() + aBonus;
}

/* this results in a compiler error */
public int getCompensation() {
  return aSalary + aBonus;
}
```

## Abstract Classes
- while inheriting code can be desirable to avoid code duplication, sometimes we _need_ subclasses to implement their own methods
  - and yet, it makes sense to force all subclasses to have the same method
  - but to have different implementations
  - for ex. both a stack and a list should have a method to get the first element, but it must be implemeted differently for each
- can solve this by declaring class as `abstract`
- eg. `public abstract AbstractFigure implements Figure`
- this has 3 main consequences
  - `abstract` classes cannot be instantiated
  - class does not need to supply implementation for all methods (only the non abstract ones)
  - class can declare `abstract` methods
    - ex. `protected abstract void draw();`

## Template Method Design Pattern
- sometimes some code is common for all subclasses except some small parts of an algorithm that vary from subclass to subclass
- solution: put all common code in superclass and define "hooks" to allow subclasses to implement specialized functionality where needed
- named so because superclass is a "template"

Let us use drawing a figure as an example. Assume drawing a figure consists of three parts:
1. clearing the canvas
2. drawing the figure
3. saving the canvas

- steps 1 and 3 should be the same no matter what figure is being drawn
- step 2 varies depending on the figure

Solution:
- create class `AbstractFigure implements Figure`
- add private method for clearing the canvas (eg. `clear()`
- add a private method for saving the canvas (eg. `save()`
- instanciate the public or protected method for drawing the figure (ie. `draw()`)
- add a specialized draw method that will be abstract for subclass implementation differences (eg. `drawFigure()`)
  - make sure it is abstract!
  - eg. `protected abstract void drawFigure()`
- call the abstract method in the `draw()` method

```java
public AbstractFigure implements Figure {
  private void clear() {...}
  private void save() {...}
  
  public void draw(Graphics pGraphics) {
    clear();
    drawFigure(pGraphics);
    save();
  }

  public abstract void drawFigure(Graphics pGraph) {
  ...
  }
}
```
- implementation for `drawFigure` will have to be implemented by each subclass
- good idea to declare template method as `final` so it cannot be overridden in subclasses
- abstract step modifier should usually be `protected`

## When to use Inheritance
- inheritance should only be used for extending the behaviour os superclasses
  - incorrect to limit or restrict behaviour of superclass
  - or use inheritance when subclass is not a proper subtype of the superclass
- example: a circle should not inherit from Ellipse because it limits Ellipses to ones that have equal proportions

### Liskov Substitution Principle
- subclasses should not restrict what clients of the superclass can do with an instance
- can use a subtype wherever a supertype is used
- part of SOLID
  - Single responsibility
  - Open/closed
  - Liskov substitution
  - Interface segregation
  - Dependency inversion
- specifically, methods of the subclass
  - cannot have stricter preconditions
  - cannot have less strict post-conditions
  - cannot take more specific types as parameters
  - cannot make the method less accessible (eg. public -> protected)
  - cannot throw more checked exceptions
  - cannot have a less specific return type
