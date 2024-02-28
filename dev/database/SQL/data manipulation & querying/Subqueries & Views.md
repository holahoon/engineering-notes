
## Subqueries

There are times when we have complex tables and need to query from another query.
Whenever we query from a table, it kind of returns a table right?

So we can run subqueries:
```sql
SELECT customer_name, product_name
FROM (SELECT * FROM sales
WHERE volume > 1000) AS base_result;
```
This example is pretty dumb, but you get the idea.
Make sure wrap with `()` for the subquery and give a name to the query (in our case `base_result`).

## Views

We can pre-define a query to re-use it. It's almost like a constant:
```sql
CREATE VIEW base_result AS SELECT * FROM sales WHERE volume > 1000;

SELECT customer_name, product_name
FROM (base_result);
```
View is stored by the database engine. It does not store the query results, but it just executes whenever the View is called.
