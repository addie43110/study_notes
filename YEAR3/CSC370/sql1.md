# SQL (1)

## Creating Tables
```SQL
CREATE TABLE Movies (
  title VARCHAR(50),
  year INT,
  length INT,
  rating CHAR(2),
  studioname VARCHAR(20)
);
```

### CHAR and VARCHAR
- CHAR(n) allocates a fixed space
- VARCHAR(n) allocates UP TO n CHARACTERS
  - only uses as much of those n characters as needed
- use CHAR in frequently used fields as it is slightly faster
- use VARCHAR otherwise

## Insert
```SQL
INSERT INTO Studios
VALUES ('Fox', 'foxmovies.com');

INSERT INTO Movies
VALUES ('Walk the Line', 2005, 136, 'PG', 'Fox');
```

## Primary keys
```SQL
CREATE TABLE Studios (
  name VARCHAR(20) PRIMARY KEY,
  webside VARCHAR(255)
);

CREATE TABLE Movies (
  title VARCHAR(50),
  year INT,
  length INT,
  rating CHAR(2),
  studioname VARCHAR(20),
  PRIMARY KEY (title, year)
);
```

## Foreign keys
```SQL
CREATE TABLE Movies (
  title VARCHAR(50),
  year INT,
  length INT,
  rting CHAR(2),
  studioname VARCHAR(20),
  PRIMARY KEY (title, year),
  FOREIGN KEY (studioName) REFERENCES Studios(name) ON DELETE CASCADE
);


--alternative
CREATE TABLE Movies (
  title VARCHAR(50),
  year INT,
  length INT,
  rating CHAR(2),
  studioname VARCHAR(20) REFERENCES Studios(name) ON DELETE CASCAE,
  PRIMARY KEY (title, year)
);
```

## Dropping tables
```sql
DROP TABLE StarsIn;
DROP TABLE Movies;
```

## Get all tuples of a table
```sql
SELECT *
FROM Movies;
```

## Modify table stucture
```sql
ALTER TABLE Stars ADD phone CHAR(7);

ALTER TABLE Stars MODIFY phone CHAR(10);

ALTER TABLE Stars DROP COLUMN phone;
```
