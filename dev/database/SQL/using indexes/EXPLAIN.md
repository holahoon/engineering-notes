
[MySQL](https://dev.mysql.com/doc/refman/5.7/en/using-explain.html)
[PostgreSQL](https://www.postgresql.org/docs/current/sql-explain.html)

`EXPLAIN` provides information about how SQL query executes statements.

```sql
EXPLAIN
SELECT * FROM users
WHERE salary > 12000;
```

We can also add `ANALYZE` to get analyzed data of our query statement.
```sql
EXPLAIN ANALYZE
SELECT * FROM users
WHERE salary > 12000;
```
