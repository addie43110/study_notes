# Structured Analysis + Domain Modeling

## Traditional 'Structured' Modeling
  - data modeling artifacts
    - entity relationship diagrams (ERDs)
    - data dictionary (DD)
  - process modeling artifacts
    - data flow diagrams (DFDs)

## Data modeling
  - entities are NOUNS
  - show relationships between entities
    - can show cardinality
  - describe each entity in the data dictionary

## Process-oriented modeling (data flow modeling)
  - boxes are VERBS (processes/functions)
  - show relationships between process bubbles (ie. the data that flows between them)
  - different levels

### DFD components
  - entity (ie. student, doctor, charge nurse, ...)
    - similar to actors in use cases
    - represent a person, a business, department, or a machine
    - a source of data or a destination
    - should be a noun
  - data flow arrows
    - shows movement of information
    - described with a noun
    - uses arrowheads
  - processes
    - similar to use cases or steps in a use case
  - data stores
    - where information is stored
    - allows examination, retrieval and adding more data
  

### DFD 0
  - context diagram
  - highest level in a DFD
  - contains only one process (the system)
  - process given number 0
  - all external entities are shown

### DFD levels
  - top level is context level
  - each level is progressively detailed
  - lower level diagram number same as the parent process number
  - processes that don't create child diagrams are called "primitive"

### Common errors in DFDs
  - incorrect labeling
  - more than 9 processes on a data flow diagram
  - diagram inconsistent with parent diagram

## Object oriented modelins
  - similar to ER diagrams but without actions on arrows
  - now we just show attributes of a class and cardinality
  - no verbs!


# Requirements Modeling
## Why?
Requirements can be difficult to capture
  - not always obvious
  - many sources
  - not easy to communicate
  - different types at different levels of detail
  - related to one another
  - change / time-sensitive

How do we deal with this?
  - do a good job of gathering requirements
  - then use models to keep track of requirements
  - show models to stakeholders for validation

## Use case modeling
  - diagrams to provide an overview of actors and use cases
  - can be done at different levels of detail
  - can be done with varying formality

### Formality
  - preconditions
  - success steps
  - post conditions
  - alternate paths

## Domain models
  - use case modeline is supported by domain modeling and UI modeling
  - DM : functional requirements
  

## UI model 
  - paper or electronic
  - model only essential parts of the user interface

### Why create UI models?
  - explore requirements with stakeholders in a visual way
  - bases for actual design


### How to create a UI model
  - should directly support one or more use cases
  - pick steps from a use case that are visible to the user
  

### Types of prototypes
  - throw away prototypes
  - evolutionary (incremental prototypes)
    - eventually becomes the final product
  - low-fidelity prototypes 
    - quick and cheap, paper based
  - medium or high-fidelity prototypes
    - devloped with a visual language
    - more sophisticated

### Storyboarding
  - a series of key frames (sketches)

### Prototypes: advantages
  - quick and cheap to produce
  - elicits detailed user interface requirements better han interviewing
  - developer gains experiences and insight
  - improves decisions

### Prototypes: disadvantages
  - can focus on design aethetics over functionality
  - false impression that developer is almost done or system is easy to build
  - customer may think any flaws in the prototype are flaws in the original design
