

We can get a record with a certain email value from a table like:
```sql
SELECT first_name, last_name
FROM customers
WHERE email = 'dk@test.com';
```
This would return us the record with the record with the matching email.

We can check if the record "exists" with the given email:
```sql
SELECT EXISTS(
	SELECT first_name, last_name
	FROM customers
	WHERE email = 'dk@test.com'
);
```
In PostgreSQL, this would return us `TRUE` / `FALSE` depending on the existence of the record, or `1` / `0` for MySQL.

We can of course use a filter and return `TRUE` or `FALSE`, but this would scan the entire table.
If we use `EXISTS`, it stops once it finds the matching condition.

## With Subquery Expressions and EXISTS

We can look for a record in `orders` table by selecting the `email` column from `customers` table.

```sql
SELECT o.id
FROM orders AS o
WHERE EXISTS(
	SELECT email FROM customers AS c
	WHERE c.id = o.customer_id AND c.email = 'dk@test.com'
);
```
It would find the matching order with customer and the email that matches `'dk@test.com'`.