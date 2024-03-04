
[PostgreSQL](https://www.postgresql.org/docs/14/functions-string.html)
[MySQL](https://dev.mysql.com/doc/refman/8.0/en/string-functions.html)

We can concatenate two string values into a one:
```sql
SELECT CONCAT(first_name, ' ', last_name) FROM memberships;

-- This works in PostgreSQL btw
SELECT first_name || ' ' || last_name FROM memberships;
```
This would give us a list of concatenated `first_name` and `last_name` value with an empty space in the middle.

We can also add a string value to a number type:
```sql
SELECT CONCAT('$ ', price) FROM memberships;
```
This will give us something like `$ 19.99` ...

## Using with `INSERT`

```sql
INSERT INTO memberships (
	membership_start,
	gender
)
VALUES (
	'2020-05-10',
	TRIM(TRAILING ' ' FROM 'male ')
);
```
`TRIM()` will remove any white spaces when inserting data.

We can check by running:
```sql
SELECT LENGTH(gender) FROM memberships;
```
