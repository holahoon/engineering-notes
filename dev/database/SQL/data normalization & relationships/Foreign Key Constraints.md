
```sql
CREATE TABLE users(
	...
	address_id INT REFERENCES addresses (id) ON DELETE CASCADE
	...
);
```

- `REFERENCES`: Defines the **related table + column**
- `ON DELETE` / `ON UPDATE`: Defines the **action** that should be executed if a related row is deleted or updated

### Adding Foreign Key Constraints via `ALTER TABLE`

You can also add foreign key constraints after a table has been created with `ALTER TABLE`.
```sql
ALTER TABLE <table_name>
ADD FOREIGN KEY <column_name> REFERENCES <related_table>
(related_column) ON DELETE ... ON UPDATE ...
```

### Removing Foreign Key Constraints via `ALTER TABLE`

You can also remove foreign key constraints:
```sql
ALTER TABLE <table_name>
DROP FOREIGN KEY <constraint_name>;
```
In order to DROP a foreign key constraint (just as for dropping any other kind of constraint), you need to assign a name to the constraint when adding it.

#### Adding with constraint name via `CREATE TABLE`:
```sql
CREATE TABLE <table_name> (
	<column_name> <column_data_type> FOREIGN KEY <constraint_name> REFERENCES ...
);
```

#### Adding with constraint name via `ALTER TABLE`:
```sql
ALTER TABLE <table_name>
ADD CONSTRAINT <constraint_name> FOREIGN KEY <column_name> REFERENCES ...
```

## ON DELETE & ON UPDATE

- `RESTRICT`: prevent the intended action (e.g. deleting a related row)
- `NO ACTION`: default. prevent the intended action (e.g. deleting a related row). Check can be deferred, e.g. as part of a transaction
- `CASCADE`: perform the same action (e.g. deleting a related row) on the row with the foreign key
- `SET NULL`: set the foreign key value to `NULL` if the related row was deleted
- `SET DEFAULT`: set the foreign key value to its `DEFAULT` value if the related row was deleted

## Example

Let's first create a table and have our users table `address_id` to reference addresses `id`.
```sql
CREATE TABLE addresses (
	-- id INT PRIMARY KEY AUTO_INCREMENT, -- MySQL
	id SERIAL PRIMARY KEY, -- Postgresql
	street VARCHAR(300) NOT NULL,
	house_number VARCHAR(50) NOT NULL,
	city_id INT NOT NULL
);

CREATE TABLE users (
	-- id INT PRIMARY KEY AUTO_INCREMENT, -- MySQL
	id SERIAL PRIMARY KEY, -- Postgresql
	first_name VARCHAR(300) NOT NULL,
	last_name VARCHAR(300) NOT NULL,
	email VARCHAR(300) NOT NULL,
	address_id INT REFERENCES addresses (id) ON DELETE CASCADE
);

CREATE TABLE cities (
	-- id INT PRIMARY KEY AUTO_INCREMENT, -- MySQL
	id SERIAL PRIMARY KEY, -- Postgresql
	name VARCHAR(300) NOT NULL
);
```

Let's insert some dummy data:
```sql
INSERT INTO cities (name)
VALUES ('Seoul'), ('New York'), ('London');
  
INSERT INTO addresses (street, house_number, city_id)
VALUES
	('Teststreet', '8A', 3),
	('Some street', '10', 1),
	('Teststreet', '1', 3),
	('My street', '101', 2);
  
INSERT INTO users (first_name, last_name, email, address_id)
VALUES
	('David', 'Kim', 'dk@test.com', 2),
	('Manuel', 'Lorenz', 'manu@test.com', 4),
	('Julie', 'Barnes', 'julie@barnes.com', 3);
```

Try deleting:
```sql
DELETE FROM addresses
WHERE id = 2;
```

Notice how in our users table, the record `('David', 'Kim', 'dk@test.com', 2)` is deleted along with `('My street', '101', 2)` from addresses table.
This is because we have set `ON DELETE CASCADE` when creating the users table.