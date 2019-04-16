# Constraints

## Primary Keys
Primary Keys are a type of constraint.
```sql
CREATE TABLE Movies (
  title CHAR(40) PRIMARY KEY,
  year INT,
  length INT,
  type CHAR(2)
);

CREATE TABLE Movies (
  title CHAR(40),
  year INT,
  length INT,
  type CHAR(2),
  PRIMARY KEY (title, year)
);
```

## NOT NULL
```sql
CREATE TABLE ABC (
  A number NOT NULL,
  B number NULL,
  C number
);

insert into ABC values (1, null, null);
insert into ABC values (2, 3, 4);
insert into ABC values (null, 5, 6); -- this throws an error
```

## Unique
- the UNIQUE constraint doesn't allow duplicate values in a column
- however, NULLs are still allowed 

```sql
CREATE TABLE AB (
  A NUMBER UNIQUE,
  B NUMBER
);

insert into AB values (2, 1);
insert into AB values (null, 9);
insert into AB values (null, 9);
insert into AB values (2, 7); -- the last statement issues an error
```

## Naming constraints
You can name your constraints ifyou wish to use them / modify them in the future.

```sql
CREATE TABLE ABC (
  A number,
  B number,
  C number,
  CONSTRAINT my_unique_constraint UNIQUE (A,B)
);
```

## Dropping and adding constraints
```sql
ALTER TABLE AB DROP CONSTRAINT my_unique_constraint;

ALTER TABLE AB ADD CONSTRAINT my_unique_constraint UNIQUE(A);
```

## Foreign Key Constraints
You can use references, or the foreign key syntax, or you can name your foreign key constraints!

```sql
CREATE TABLE Emp (
  empno INT PRIMARY KEY,
  deptno INT REFERENCES Dept(deptno)
);

CREATE TABLE Emp (
  empno INT PRIMARY KEY,
  deptno INT,
  FOREIGN KEY(deptno) REFERENCES Dept(deptno)
);

CREATE TABLE StarsIn (
  title VARCHAR2(40),
  year INT,
  starName VARCHAR2(20),
  CONSTRAINT fk_movies FOREIGN KEY(title, year) REFERENCES Movies(title, year),
  CONSTRAINT fk_moviestars FOREIGN KEY(starName) REFERENCES MovieStars(name)
);
```

## Foreign key constraints with nulls
A foreign key value must either
1. appear as a primary key value in the parent table or
2. be null

If we don't want NULL's in a foreign key we must say so.
```sql
CREATE TABLE Project (
  pno INT PRIMARY KEY,
  pmno INT NOT NULL REFERENCES Emp,
);
```

Foreign key constraints can also refer to the same table!
  - aka parent = child

```sql
CREATE TABLE Emp (
  empno INT PRIMARY KEY,
  mgrno INT NOT NULL REFERENCES Empy,
);
```

## Chicken and egg
```sql
CREATE TABLE chicken (
  cID INT PRIMARY KEY,
  eID INT REFERENCES egg(eID)
);

CREATE TABLE egg (
  eID INT PRIMARY KEY,
  cID INT REFERENCES chicken (cID)
);
```

But if we simply type the above statements, we'll get an error.
  - the reason is because the `CREATE` statement for the chicken table refers to table egg, which hasn't been created yet!
  - creating egg doesn't solve this either because egg refers to chicken similarly

### A first attempt
Plan:
- create the chicken and egg tables without the fk constraints
- alter table to add the foreign key constraints after

```sql
CREATE TABLE chicken (
  cID INT PRIMARY KEY,
  eID INT
);

CREATE TABLE egg (
  eID INT PRIMARY KEY,
  cID INT
);

ALTER TABLE chicken ADD CONSTRAINT chickenREFegg
FOREIGN KEY (eID) REFERENCES egg(eID);

ALTER TABLE egg ADD CONSTRAINT eggREFchicken
FOREIGN KEY (cID) REFERENCES chicken(cID);
```

However, this approach leads to the problem of inserting tuples.
- we cant insert into chicken first because it requires a reference to egg
- we can't insert into egg first because it requires a reference to chicken

### Second attempt using deferrable constraints
To solve the problem, we can use deferrable constraints
- we need the ability to group several SQL statements into one unit called a **transaction**
- then we need a way to tell SQL not to check the constraints until the transaction is committed

Any constraint may be declared "DEFERRABLE" or "NOT DEFERRABLE"
  - NOT DEFERRABLE is the default, and means that every time a database modification occurs, the constraint is immediately checked
  - DEFERRABLE means that we have the option of telling the system to wait until a transaction is complete before checking the constraint

Similarly, there are two other options called `INITIALLY DEFFERRED` and `INITIALLY IMMEDIATE`
- INITIALLY DEFERRED: the check will be deferred to the end of thecurrent transaction
- INITIALLY IMMEDIATE (default): the check will be made before any modification
  - but, because the constraint is deferrable, we have the option of later deciding to defere checking (`SET CONSTRAINT MyConstraint DEFERRED)`

```sql
ALTER TABLE chicken ADD CONSTRAINT checknREFegg
FOREIGN KEY (eID) REFERENCES egg(eID)
DEFERRABLE INITIALLY DEFERRED;

ALTER TABLE egg ADD CONSTRAINT eggREFchicken
FOREIGN KEY (cID) REFERENCES chicken(cID)
DEFERRABLE INITIALLY DEFERRED;
```

## Successful insertions
Now we can finally insert values.

```sql
INSERT INTO chicken VALUES (1, 2);
INSERT INTO egg VALUES (2, 1);
COMMIT;
```

## Dropping constraints
```sql
ALTER TABLE egg DROP CONSTRAINT eggREFchicken;
ALTER TABLE chicken DROP CONSTRAINT chickenREFegg;

DROP TABLE egg;
DROP TABLE chicken;
```

## Check constraints
Allows users to restrict possible attribute values for columns to admissible ones.
  - aka allows constraints on attribute values beyond just "NULL", like "can't be 0" or "must be greater than 1"

Example
- name of an employee must be ALL CAPS
- minimum salary of an employee is 500
- department numbers must range between 10 and 100

```sql
CREATE TABLE Emp (
  empno NUMBER,
  ename VARCHAR2(30) CONSTRAINT check_name CHECK (ename = UPPER(ename)),
  sal NUMBER CONSTRAINT check_sal CHECK (sal >= 500),
  deptno NUMBER CONSTRAINT check_deptno CHECK(deptno BETWEEN 10 AND 100)
);
```

## Checking 
- the DBMS automatically checks the specified conditions each time a database modification is performed on this relation

Example
```sql
INSERT INTO emp VALUES (7999, 'SCOTT', 450, 10); -- this is rejected
```

- a check constraint can also be a table constraint, and the condition can refer to any column of the table
- for example: a project's start date must be before the end date

```sql
CREATE TABLE Project (
  pstart DATE,
  pend DATE,
  CONSTRAINT dates_ok CHECK (pend > pstart)
);
```

## Violating Tuples
Suppose on the employee table, we dropped the constraint that checks salaries. Then we added the following tuple:

```sql
INSERT INTO emp(empno, ename, sal, deptno) VALUES (9, 'ALEX', 300, 20);
```

Next, we altered the table to re-add the constraint, but this time putting exceptions into a table named Exceptions.

```sql
ALTER TABLE Emp ADD CONSTRAINT check_sal CHECK (sal >= 500)
EXCEPTIONS INTO Exceptions;

-- the constraint cannot be created because there is a violating tuple.
-- however, the violating tuples can be collected in the Exceptions table.
```

To identify the tuples that violate a constraint, one can use the clause:
```sql
EXCEPTIONS INTO Exceptions
```

where `Exceptions` is a table that we would have to create and which stores information about violating tuples.

Let's create the table Exceptions.

```sql
CREATE TABLE Exceptions (
  row_id ROWID,
  owner VARCHAR2(30),
  table_name VARCHAR2(30),
  constraint VARCHAR2(30)
);
```

Each tuple has to have a column rowid that is used to identify tuples. 
- row_id will reference the rowid in the Emp table
- besides row_id, the name of the table, the table owner, as well as the name of the violated constraint are stored

Then, we can query the exceptions.
```sql
SELECT Emp.*, constraint
FROM Emp, Exceptions
WHERE Emp.rowid = Exceptions.row_id;
```

## Writing constraints correctly
Since we can't use implications, we must change implications into terms of "OR".

Example
- create table MovieStar. If the star gender if 'M', then his name must not begin with 'Ms.'
- so 'M' -> not 'Ms'
  - or not 'M' OR 'Ms'

```sql
CREATE TABLE MovieStar (
  name CHAR(20) PRIMARY KEY,
  gender CHAR(1),
  CHECK (gender <> 'M' OR name NOT LIKE 'Ms.%')
);


