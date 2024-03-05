
Indexes can enhance query performance.
In index table, values are stored and sorted. A connection to the original row is stored along with the index value.
But, Whenever index values change, the values have to be updated in both the original table AND the index table.
If we have many indexes, many tables are updated whenever data is manipulated. This can be very inefficient.
Also, if we have frequent value changes, multiple tables are updated frequently.

Therefore, you want to be careful and use indexes for columns which you **a lot** in WHERE clauses, but you don't necessarily update too often.

## Different Types of Indexes

There are different types of technical implementation.
- B-Tree
- Hash
- GiST
- ...
These use different algorithms and support different comparison operations

There are different kind of index / functionality.
- Standard single-column index
- Unique index
- Multi-column index (composite index)
- Partial index (not available for MySQL)
These are used for different purposes & data values

## INDEX

### Create an index

You can typically create index after creating a table.
```sql
CREATE INDEX salaryidx ON users (salary);
```
Here, we create an index called `salaryidx` on `users` table for the `salary` column.

You can also create an index right when creating a table:
```sql
CREATE TABLE <table_name> (
	... column definitions,
	INDEX <index_name> (<column>)
);
```
💥 Just note that it is supported in MySQL, but not in PostgreSQL.

### Delete an index

```sql
DROP INDEX salaryidx;
```

## Unique Indexes

We know how `UNIQUE` works when creating a table:
```sql
CREAT TABLE users (
	id SERIAL PRIMARY KEY,
	...,
	email VARCHAR(300) UINQUE NOT NULL
);
```
Our `email` column will be unique.
This `UNIQUE` keyword also creates an index for that column as well!

We can also create a unique index when creating an index, after the table is created:
```sql
CREATE UNIQUE emailidx ON users (email);
```
This will create a unique index that also check there are no duplicated values for that column.

## Composite Indexes

We can create a composite index:
```sql
CREATE INDEX multiaddress ON addresses (street, city);
```
The order matters!

We can try to get how the query ran:
```sql
EXPLAIN ANALYZE
SELECT * FROM addresses
WHERE street = 'Teststreet' AND city = 'Munich';
```
Index lookup will not work if we use `OR`.

## Partial Indexes

We can create a partial index.
This is helpful if we know for sure that we are going to run queries on certain conditions.

```sql
CREATE INDEX salaryidx ON users (salary)
WHERE salary > 12000;
```
This will create indexes on `salary` that is greater than 12000.

⚠️ Not available on MySQL.
