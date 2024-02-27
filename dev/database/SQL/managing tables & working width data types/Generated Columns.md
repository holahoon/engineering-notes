
There could be times when we want to generate a column when creating a table.

Say we have `first_name` and `last_name` column and we want to auto-generate a `full_name` with these two columns.

This is specifically for MySQL:
```sql
CREATE TABLE users (
	id INT PRIMARY KEY AUTO_INCREMENT,
	first_name VARCHAR(200) NOT NULL,
	last_name VARCHAR(200) NOT NULL,
	full_name VARCHAR(400) GENERATED ALWAYS AS (CONCAT(first_name, ' ', last_name)),
	yearly_salary INT CHECK (yearly_salary > 0),
	current_status employment_status
);

INSERT INTO users (first_name, last_name, yearly_salary, current_status)\
VALUES ('Myung Hoon', 'Kim', 5000, 'employed');
```
This will create `full_name` column with a value of `Myung Hoon Kim`.

btw, PostgreSQL is a bit restricted with this feature.
