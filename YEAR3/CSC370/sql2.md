# SQL (2)

## Select-From-Where statements
```sql
SELECT desired attributes
FROM one or more tables
WHERE condition about tuples of the tables

SELECT title AS name, length/60.0 AS length, 'hrs.' AS inHours
FROM Movies
WHERE studioName = 'Disney';
```

### Boolean operators
```sql
AND
OR 
NOT
```

Order of precedence:
1. NOT
2. AND
3. OR

### Patterns
```sql
LIKE -- <Attribute> LIKE <pattern>
NOT LIKE -- <Attribute> NOT LIKE <pattern>
% -- "any string"
_ -- "any character"
```

Example: suppose we remember a movie called "Princess _something_"
```sql
SELECT title
FROM Movies
WHERE lower(title) LIKE '%princess%'; -- lower means toLowerCase
```

### Order by
- ordering is ascending unless you specify the DESC keyword after an attribute
