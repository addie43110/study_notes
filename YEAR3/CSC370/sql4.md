# TOP N QUERY

## In Oracle
```sql
SELECT *
FROM Movies
WHERE rownum <=5;
```

`rownum` is a hidden column that sequentially numbers the tuples

## In MySQL, PostgreSQL, SQLite
```sql
SELECT *
FROM Movies
LIMIT 5;
```
## In PostgreSQL (newer version) and IBM DB2
```sql
SELECT *
FROM Movies
FETCH FIRST 5 ROWS ONLY;
```

## In Microsoft SQL Server
```sql
SELECT TOP 5 *
FROM Movies;
```

