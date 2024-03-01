 
## One-to-Many

`1:n`
One record in table A has one or many related records in table B.
e.g. an employee belongs to one company but a company has many employees.

## Many-to-Many

`n:n`
One record in table A has one or many related tables in table B - and vice versa.
e.g. an employee is part of multiple projects and every project has multiple employees assigned to it.

## One-to-One

`1:1`
One record in table A has one related table in table B - and vice versa.
e.g. an employee has exactly one intranet account and every intranet account belongs to exactly one employee.

## Example

[[data-relationship-diagram.canvas|data-relationship-diagram]]

### Many-to-Many relations need intermediate tables

We can't add foreign keys when dealing with `n:n` tables. So, we need an "intermediate table" to store the relations between two tables.

### Create tables!

This table is for PostgreSQL.

```sql
CREATE TABLE projects (
	id SERIAL PRIMARY KEY,
	title VARCHAR(300) NOT NULL,
	deadline DATE
);

CREATE TABLE company_buildings (
	id SERIAL PRIMARY KEY,
	name VARCHAR(300) NOT NULL
);

CREATE TABLE teams (
	id SERIAL PRIMARY KEY,
	name VARCHAR(300) NOT NULL,
	building_id INT REFERENCES company_buildings (id) ON DELETE SET NULL
);

CREATE TABLE employees (
	id SERIAL PRIMARY KEY,
	first_name VARCHAR(300) NOT NULL,
	last_name VARCHAR(300) NOT NULL,
	birth_date DATE NOT NULL,
	email VARCHAR(200) UNIQUE NOT NULL,
	team_id INT DEFAULT 1 REFERENCES teams (id) ON DELETE SET DEFAULT
);

CREATE TABLE intranet_accounts (
	id SERIAL PRIMARY KEY,
	email VARCHAR(200) REFERENCES employees (email) ON DELETE CASCADE,
	password VARCHAR(200) NOT NULL
);

-- Intermediate table => n:n
CREATE TABLE projects_employees (
	id SERIAL PRIMARY KEY,
	employee_id INT REFERENCES employees (id) ON DELETE CASCADE,
	project_id INT REFERENCES projects (id) ON DELETE CASCADE
);
```
The order of the table matters when working with table relationships.
- `intranet_accounts` has a 1:n relationship with `emyployees` table. One employee can have many intranet accounts. The foreign key in this case is the `email` field. Notice `employees` has `UNIQUE` for `email` column because `email` must be unique for each employee.
- `employees` table has a 1:n relationship with `teams` since one employee can be in multiple teams. The foreign key in this case is using the `id` primary key in `teams` table.
- `teams` has a 1:n relationship with `company_buildings` table with foreign key of `building_id` which references to `company_buildings` table's `id` column.
- `projects` has n:n relationship with `employees`. This can only be achieved using intermediate table, which in our case the `projects-employees` table.

### Insert some dummy data

```sql
INSERT INTO company_buildings (name)
VALUES
	('Main Building'),
	('Research Lab'),
	('Darkroom');

INSERT INTO teams (name, building_id)
VALUES
	('Admin', 1),
	('Data Analysts', 3),
	('R&D', 2);

INSERT INTO employees
	(first_name, last_name, birth_date, email, team_id)
VALUES
	('Julie', 'Barnes', '1988-10-01', 'julie@test.com', 3),
	('Max', 'Schwarz', '1989-06-10', 'max@test.com', 1),
	('Manuel', 'Lorenz', '1987-01-29', 'manu@test.com', 2),
	('Michael', 'Smith', '1977-05-12', 'michael@test.com', 2);

INSERT INTO intranet_accounts (email, password)
VALUES
	('max@test.com', 'abcae1'),
	('manu@test.com', 'fdasfdas1'),
	('julie@test.com', 'adsfsaf3'),
	('michael@test.com', 'adsfsaf3');

INSERT INTO projects (title, deadline)
VALUES
	('Mastering SQL', '2024-10-01'),
	('New Hire Onboarding', '2022-02-28'),
	('New Course Evaluation', '2022-01-01');

INSERT INTO projects_employees (employee_id, project_id)
VALUES
	(1, 2),
	(2, 2),
	(1, 3),
	(3, 1),
	(3, 3),
	(2, 3);
```

### Play around with querying data

```sql
-- SELECT e.id AS employee_id, e.first_name, e.last_name, p.title FROM employees AS e
-- LEFT JOIN projects_employees AS pe ON pe.employee_id = e.id
-- LEFT JOIN projects AS p ON pe.project_id = p.id;
  
-- SELECT e.id AS employee_id, e.first_name, e.last_name, p.title FROM employees AS e
-- INNER JOIN projects_employees AS pe ON pe.employee_id = e.id
-- LEFT JOIN projects AS p ON pe.project_id = p.id;

-- SELECT e.id AS employee_id, e.first_name, e.last_name, t.name
-- FROM employees AS e
-- INNER JOIN teams AS t ON t.id = e.team_id
-- WHERE t.id = 2;

SELECT e.id AS employee_id, e.first_name, e.last_name, cb.name
FROM employees AS e
INNER JOIN teams AS t ON t.id = e.team_id
INNER JOIN company_buildings AS cb ON cb.id = t.building_id
WHERE cb.id = 3
ORDER BY e.birth_date;
```
