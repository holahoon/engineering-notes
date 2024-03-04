
[PostgreSQL](https://www.postgresql.org/docs/9.5/functions-math.html)
[MySQL](https://dev.mysql.com/doc/refman/8.0/en/mathematical-functions.html)

There are a lot of mathematical operators, but here are some examples:

## Mathematical Functions
### Round Up
```sql
SELECT CEIL(consumption) FROM memberships;
```
We can pass in a second argument to specify the decimal point
### Round down
```sql
SELECT ROUND(consumption, 1) FROM memberships;
```
We can pass in a second argument to specify the decimal point
### Truncate
```sql
-- PostgreSQL
SELECT TRUNC(consumption, 1) FROM memberships;
-- MySQL
SELECT TRUNCATE(consumption, 1) FROM memberships;
```
