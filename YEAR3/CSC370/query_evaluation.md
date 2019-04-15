# Query Evaluation

## An SQL query and it's relational algebra equivalent

<span style="color:red">Employees</span>(sin INT, ename VARCHAR(20), rating INT, address VARCHAR(90))<br/>
<span style="color:red">Maintenances</span>(sin INT, planeId INT, day DATE, desc CHAR(120))

```sql
SELECT ename
FROM Employees NATURAL JOIN Maintenances
WHERE planeId = 100 AND rating > 5;
```

![ra](images/raquery.png)

- RA expressions can be and are represented by an expression tree
- an algorithm is chosen for each node in the expression tree

![expression tree](images/exptree.png)

## Query optimizaion
- SQL queries are translated into RA
- query evaluation plans are represented as trees of relational operators
  - with labels identifying the algorithm to use at each node
- initial trees can be transformed to "better" trees
  - this process of finding good evaluation plans is called query optimization

## Clustering / non-clustering indexes
- <span style="color:red">clustering index (or clustered):</span> tuples are stored in the same order as the key-value pairs in the index
  - same as "primary"
  - typically B-Tree with records in the leaves
- <span style="color:red">non-clustering index (or non-clustered):</span> tuples are stored randomly, not controlled by the index
  - same as "secondary"
  - typically B-Tree with key-pointers in the leaves (not the records themselves)

## Airline example
- Assume for Maintenances that
  - a tuple is 160 bytes
  - a block can hold 100 tuples (16K block)
  - we have 1000 blocks of such tuples
- Assume for employees
  - a tuple is 130 bytes
  - a block can hold 120 tuples
  - we have 50 blocks of such tuples

## Algorithms for selection
Suppose we want to select some relationship attribute, R.attr, when it is equal to some value.

![selection](images/selection.png)

- if no index is on R.attr, just scan R
  - on average, R occupies half the number of blocks allocated to it
    - eg. if R is maintenances, there will need to be 1000/2 block reads (I/Os)
- if there is an index on R.attr, use the index
  - typically this will take 3 disk accesses
  - assumes non-clustering B-Tree with 3 levels and the root in main memory


