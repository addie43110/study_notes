# Visitor Design Pattern
- define a new operations without changing the classes of the elements on which it operates
- uses double dispatch

## Purpose
- abstract functionality that can be applied to an aggregate hierarchy of "element" objects

## Implementation
- create a Visitor class with a `visit()` method
- ???

## Example
- person calls a taxi company (taxy company accepts a visitor)
- company dispatches a cab to the customer
- upon entering the taxi, the cusomter (or visitor) is no longer in control of his/her own transportation

![visitor example](images/visitor.png)
