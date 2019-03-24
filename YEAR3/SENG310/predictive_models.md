# Analytics and Predictive Models

## Analytics
- a set of methods for evaluating user traffic through a system
- not only useful for evluating the usability of a website, they are also valuable in business planning
- users are not directly involved, but their behavior is tracked


### Social network analytics
- the most common way of understanding social networks is through graphs
- graph visualization and analysis tools can provide insights about communities, clusters, statistics about users
- lifelogging includes recording GPS location data and all interactions while using a cellphone
- privacy implications

## You are not a gadget
- the futility of hoping that new mediums will increase democratization, free exchange of ideas
- in all cases, a period of giddy euphoria and utopianism is replaced by business and advertising as usual
- Jaron Lanier's book "You are not a gadget" explores this topic in detail and how companies like Google, Facebok, and Amazon are taking advantage of users

## A/B testing
- split users into two groups
- each group receives a different configuration (typically users are not informed of the switch)
- logging and analytics are used to decide which one is better and why
- external A/B testing is done in the field and both configurations are working produts
- internal A/B testing is done for products before they hit the market

### A/B testing discussion
- what are some of the key advantages of A/B testing compared to other methods of evaluation?
- what are some of the challenges /disadvantages?
- what are some of the factors that enable A/B testing?
- who can conduct A/B testing?

## CrowdSourcing
- galaxy type classification
- amazon mechanical turk
- issues:
  - validation
  - skill
  - relevance

## Predictive models
- similar to inspection and analytics, predictive models evaluate a system without users being present
- use formulas to derive various measures of user performance
  - eg. optimal layout of icons on a smart phone
- two influential HCI models:
  - Fitt's Law
  - GOMS model (sort of obsolete)

### Fitt's Law
- predict the time it takes to reach a target using a pointing device
- model the relationship between speed and accuracy when moving towards a target on a display
- time to select objects on a screen
- examples include desktop/mobile layouts, as well as touch screen interfaces

T = k log (D/S + 1.0) where:
- T = time to move the pointer to a target
- D = distance between the pointer and the target
- S = size of the target
- k = constant of approximately 200ms/bit

Introducing a label below each tool can increase target size, but also can increase the danger of accidental pressing.

### GOMS
stands for
- Goals
  - symbolic structures that define a state to be achieved
- Operators
  - elementary perceptual, motor, or cognitive acts
- Methods
  - procedure for accomplishing a goal
- Selection rules
  - there many be more than one method available to the user to accomplish a goal, so the user will use selection rules to choose one

Used for:
- producing quantitative and qualitative predictions of how people will use a proposed system
- methods that are used to achieve specific goals



## Subjective assessment of audio/image quality

Basic idea:
1. Calculate features of the media
2. Collect subjective impressions from user study
3. Develop model that predicts the subjective impression from the automatic features
4. Reference vs non-reference variant
