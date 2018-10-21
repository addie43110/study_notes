# Requirements
  - describe the problem
  - what needs to be supported by the solution system

Example:
  - requirement: "prevent access to unauthorized personnel"
  - domain properties: "only a manager can assign access authority"
  - specification: "when the user enters a valid password, the computer will unlock the door"

# Problem domain vs requirements
  - problem domain: part of the universe within which the problems exist
  - requirements: what the client wishes the problem domain was like

# More examples:
  - requirement: "the lift doors are to be cycled every time that a list stops at a floor"
  - domain property: "when a lift is within 20cm vertically (above or below) of the sensor's nominal position the sensor sends a hi signal, otherwith a lo signal"
  - specification: "the system will cycle the lift doors every time a list stops at a floor"

# Types of requirements
  - functional
  - non-functional

## Functional
  - what will the system do?
  - when will it do it?
  - are there several modes of operation?
  - how and when can the system be changed?

## Non-functional
  - are there constraints on execution speed, response time, etc.?

## Quality assurance
  - reliability, availability, maintainability, security?
  - mean time between failures
  - maximum time allowed for restart

## Security
  - should access be controlled?
  - isolate user data?
  - how often should we back the system up?

## Physical environment
  - where is the equipment to function?
  - what locations?
  - environmental restrictions (ie temperature, humidity, ...)

## Interfaces
  - is the input coming from one or more systems?
  - where is the output going?
  - prescribed way the data must be formatted?

## Users and human factors
  - who will use the system?
  - different types of users?
  - training required?

## Documentation
  - is documentation required?
  - online, book ?

## Data
  - for input and ouput, what should format be?
  - how often send/receive data?
  - how accurate must the data be?
  - how much data?

## Resources
  - what materials, personnel are required to build, use, and maintain the system?
  - what skills must the developers have?
  - how much physical space will be taken up by the system?
  - budget

## Design constraints
  - anything that devs need to take into account, but user's don't need to know
  - eg. "You need to implement this for iOS."
  - eg. "Needs to be build using three main modules."
  - eg. "You must use Python library x "

