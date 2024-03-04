
A basic query like:
```sql
SELECT id
FROM customers
WHERE first_name = 'DK' OR first_name = 'David';
```
Checks for `first_name` value of either `'DK'` or `'David'`.

Using `IN` operator, we can simplify:
```sql
SELECT id
FROM customers
WHERE first_name IN('DK', 'David');
```
Which returns the same value.

We can also use
```sql
...
WHERE first_name NOT IN('Sohyeon', 'Jinho');
```
to get values that do NOT match.

## With Subquery Expressions

Let's say we want to find which customers have placed orders.

```sql
SELECT email FROM customers as c
INNER JOIN orders AS o ON o.customer_id = c.id;
```
Here, we would find all the orders that each customers placed.
This would give us a list of customer `email` that matches the `orders`' `customer_id` field.

With `IN`, we can do something like:
```sql
SELECT email
FROM customers
WHERE id IN(
	SELECT customer_id
	FROM orders
);
```
This would look for all the `email` columns in `customers` table where `id` from `customers` table matches the returned `orders`' `customer_id` column.
