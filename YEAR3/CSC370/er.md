# Entity-Relationship Models

## Components
Component | Shape
-----
Entity sets | rectangle
Attributes | oval
Relationships | diamond

## Multiplicity of Relationships
- many to one: ---<>-->
  - arrow says: "at most one"
  - eg. a movie is owned by at most one studio
- one to one: <----<>---->
- many to many: ----<>----

![multiplicity](images/multiplicity.png)

## Binary relationships
Sometimes binary relationship aren't enough. In a Movies-Studios-Stars relationship, a ternary relationship would be better. If only a binary relationship, then two or more studios could collaborte to produce only one movie. 

![binaryrel](images/binary_rel.png)

The solution is to use a ternary (three-way) relationship.

![ternary](images/ternary.png)

## Attributes
Attributes can extend off of entity sets or relationships. For example, the contracts relationship in the image above might have a "salary" attribute.

## Relationship roles
An entity set can appear more than once in a relationship. Each line to an entity set represents a different role.

![roles](images/roles.png)


