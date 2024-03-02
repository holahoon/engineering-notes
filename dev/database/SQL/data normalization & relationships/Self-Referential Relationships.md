
Self-referential is a data entity that has a relationship to itself (.i.e. internal relationship).

Let's take an example of an employee with supervisors.
We can have an employee that has a supervisor, but the supervisor is also an employee.

### Create a table

Like explained above, we can set a self-referential relationships.
We can set `supervisor_id` that references its own table's `id` column.

```sql
CREATE TABLE employees (
	id SERIAL PRIMARY KEY,
	first_name VARCHAR(300) NOT NULL,
	last_name VARCHAR(300) NOT NULL,
	supervisor_id INT REFERENCES employees (id) ON DELETE SET NULL
);
```

### Insert data

We can try to insert data. Here, we set 3 records. David will be the supervisor with `id` of 1, Jinho and Sohyeon will reference that `id` 1. Basically David is the supervisor of both people.

```sql
INSERT INTO employees (first_name, last_name, supervisor_id)
VALUES
	('David', 'Kim', NULL),
	('Jinho', 'Kim', 1),
	('Sohyeon', 'Kim', 1);
```

### Join table

We can also join its own table as well.
Here, we join `employees` table with `employees` table where the `supervisor_id` is same as `id`.

```sql
SELECT *
FROM employees AS e1
INNER JOIN employees AS e2 ON e1.supervisor_id = e2.id;
```
