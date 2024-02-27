If we need to have a column in our table that can be `NULL`, but with condition.

## When creating a table

For example, a `yearly_salary` column must be greater than 0.

We can use the `CHECK` constraint:
```sql
CREATE TABLE users (
	full_name VARCHAR(300) NOT NULL,
	yearly_salary INT CHECK (yearly_salary > 0)
);
```
Here, `yearly_salary` can be `NULL`, but when value is stored, it must be greater than 0.

We can of course use multiple conditions:
```sql
CREATE TABLE users (
	full_name VARCHAR(300) NOT NULL,
	yearly_salary INT CHECK (yearly_salary > 0 AND yearly_salary < 100000)
);
```
Here, `yearly_salary` must be greater than 0, and less than 100,000.

We can also reference another column:
```sql
CREATE TABLE users (
	full_name VARCHAR(300) NOT NULL,
	yearly_salary INT CHECK (yearly_salary > 0),
	max_salary INT DEFAULT(300000),
	CHECK (yearly_salary < max_salary)
);
```
Here, it is a table-wide check.

## When updating a table

```sql
ALTER TABLE users
ADD CONSTRAINT yearly_salary_positive CHECK (yearly_salary > 0);
```
Here, we are just giving the constraint command a "name" as `yearly_salary_positive`. This is required, but not necessary (I guess?).

Note, that this could result violation if there is a field that already has 0 in `yearly_salary` column. Obviously right?
In this case, we can simply run like:
```sql
UPDATE users
SET yearly_salary = NULL
WHERE full_name = 'Jin Ho Kim'
;

ALTER TABLE users
ADD CONSTRAINT yearly_salary_positive CHECK (yearly_salary > 0);
```
It just says to update `users` table to set `yearly_salary` to `NULL` where `full_name` is `'Jin Ho Kim`. Then modify the table.

## Column Constraints & Table Constraints

When creating a constraint for a column, we specify:
```sql
...
salary INT CHECK (salary > 0)
...
```

When creating a constraint for the table, we specify as if defining a new column:
```sql
...
salary INT,
CHECK (salary > 0)
...
```
