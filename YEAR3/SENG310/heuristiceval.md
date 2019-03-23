# Evaluation: Heuristic Evaluation and Walkthroughs 

## Inspections
Inspections are "discount" methods for testing when users are not readily available. These include:
  - heuristic evaluation
  - cognitive walkthrough

## Heuristics
General definition
  - any approach to problem solving, learning, or discover that employs a practical method not guaranteed to be optimal or perfect, but sufficient enough
  - can be mental shortcuts

Applied to UI
  - guided set of rules based on usability principles; used to help make evaluation decisions about an interface's design

### Heuristic evaluation
- the goal is to find usability issues in the interface design
- used to evaluate if the interface conforms to the design principles
- used as judging aspects of the interface as the experts perform their evaluation

### Neilsen's Heuristics
1. Visibility of system status
  - is the user always informed about what's going on in a reasonable amount of time?
2. Match between system and the real world
  - does the system use concepts and terms the user is familiar with rather than system-oriented lingo?
3. User control and freedom
  - is the user able to recover from mistakes or leave an unwanted state without extended dialog? Does the system support undo and redo?
4. Consistency and standards
  - does the interface follow standard conventions throughout? Do different words or actions mean the same thing (or vice versa)?
5. Error prevention
- does the interface protect the user against errors through constraints and other measures? Or does the interface confirm action for things that may be unsafe? 
6. Recognition rather than recall
  - does the interface reduce cognitive load by making objects and actions visible?
  - Is the user required to remember information from one step to the next?
  - Are the instructions clear?
7. Flexibility and efficiency of use
  - does the interface provide accelerators to help expert users? Are users able to tailor frequent actions?
8. Aesthetic and minimalist design
  - does the interface contain irrelevant or rarely needed information?
  - are there any elements you can remove with minimal consequences to the interaction?
9. Help users recognize, diagnose, and recover from errors
  - are the error messages clear and helpful?
  - does the interface suggest possible solutions?
10. Help and documentation
  - does the system provide clear, concise, and easily searchable documentation?

### Using heuristics
How many heuristics should I use in my evaluation?
  - experts say use between 5 and 10
  - but depends on experience
    - consider cognitive load; using too many heuristics can put too much pressure on trying to keep track of them all

How many evaluators should I use?
  - this follows the idea of diminishing returns
  - more is better, but at a certain point the increase in value trails off
  - this is usually aroung 10 and 15 evaluators

## The Process
1. Briefing
2. Evaluation period
3. Debriefing

This is not a panacea - these steps should be used with other testing methods

## Designing heuristics
Shniederman's 8 golden rules
1. Strive for consistency
2. Cater to universal usability
3. Offer informative feedback
4. Design dialogs to yield closure
5. Prevent errors
6. Permit easy reversal of actions
7. Support internal locus of control
8. Reduce short-term memory load

## Cognitive walkthroughs
- simulating a user's problem-solving process at each step in the human-computer dialog
- formalized way of imagining people's thoughts and actions when they use an interface for the first time
- cognitive walkthroughs focus on the task (main difference from heuristic evaluation)

### The process
- evaluators walk through use cases
- use scenarios to establish a context
- use personas to establish the goals of the user
- at each step, try to answer the following:
  - consider the user's goal and what they may be thinking about (intent)
    - will users know what to do to achieve the task?
    - will they see the control for their action?
  - consider the visibility of required interface elements and clarity of labels and prompts
    - once users find the control, will they recognize that it produces the effect they want?
    - will the users understand from the interface feedback whether the action was correct or not?
- walkthrough the interfaces multiple times
  - record critical information from each step
    - assumptions about what would cause the problem
    - notes about side issues and design changes
    - summarize your findings
- ask yourself: why is this not ideal?

### Common mistakes
  - don't have a clear idea of the actions involved
  - lack background knowledge or context of users
  - managers treat it too much like a formal evaluation and not enough as a design tool

### Cognitive walkthrough characteristics
- focuses on identifying specific user problems at a high level of detail
- narrow focus useful for evaluating certain aspects of a system tather than the whole thing
- cons: time consuming, laborious, and evaluators need an understanding of cognitive processes involved

## What HE and CW have in common
- uncover many usability problems
- not user testing
- user's point of view
- multiple evaluators are best
- inspections can be more thorough than user testing

## Hybrid approach: HE + CW
- perform HE with a specific task scenario in mind
- look for and identify problems at each step of the task based on a set of heuristics

## Other methods of inspection
- pluralistic walkthroughs
  - users, designers and developers come together to perform step by step analysis of a task
- analytics
  - a method for evaluating user traffic through a system
  - often used to find pain points for users
  - where do users get stuck and leave the site?
- predictive modeling (eg. Fitt's Law)

## Other things to look for during inspections
Common efficiency traps
  - switching input medium (from keyboard to mouse, etc)
- screen changes - orientation takes time
- asking user to recall from memory

## Concluding thoughts
Heuristic evaluation, cognitive walkthroughs and the application of principles are only useful if you are designing the right thing in the first place
