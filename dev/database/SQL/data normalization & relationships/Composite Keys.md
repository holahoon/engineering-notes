## Composite Primary Key

We know that we must only have one primary key per table.
There's nothing wrong with using surrogate keys, but we can have cases that we have "real key". And we can use combination of columns as a primary key.

We can define a **Composite Primary Key** which is a combination of multiple columns.

```sql
CREATE TABLE <table_name> (
	...
	employee_id INT,
	project_id INT,
	PRIMARY KEY (employee_id, project_id)
);
```

## Composite Foreign Key

Just as we can create a composite primary key, we can also create a composite foreign key.

```sql
CREATE TABLE projects_employees (
	employee_id INT,
	project_id INT REFERENCES projects (id) ON DELETE CASCADE,
	PRIMARY KEY (employee_id, project_id),
	FOREIGN KEY (employee_id) REFERENCES employees (id) ON DELETE CASCADE,
	FOREIGN KEY (employee_id, project_id) REFERENCES employees (eid, pid) ...
);
```
Here, the foreign key of `employee_id` is specified which references `employees` table's `id` column. We can also just stick with assign a foreign key to a column just like `project_id`.
Also, We can also combine foreign keys like `(employee_id, project_id)` and it references the `employee` table which this table could have a combination of columns that makes up the related values like `(eid, pid)`.