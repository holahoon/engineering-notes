
Selects and joins all the matching data from multiple tables. ONLY matching data

## Let's create a sample database:

```sql
CREATE DATABASE relations;
```

### Then let's create 3 tables: `users`, `addresses`, `cities`:

```sql
CREATE TABLE users (
	-- id INT PRIMARY KEY AUTO_INCREMENT, -- MySQL
	id SERIAL PRIMARY KEY, -- Postgre
	first_name VARCHAR(300) NOT NULL,
	last_name VARCHAR(300) NOT NULL,
	email VARCHAR(300) NOT NULL,
	address_id INT NOT NULL
);

CREATE TABLE addresses (
	-- id INT PRIMARY KEY AUTO_INCREMENT, -- Mysql
	id SERIAL PRIMARY KEY, -- Postgresql
	street VARCHAR(300) NOT NULL,
	house_number VARCHAR(50) NOT NULL,
	city_id INT NOT NULL
);

CREATE TABLE cities (
	-- id INT PRIMARY KEY AUTO_INCREMENT, -- Mysql
	id SERIAL PRIMARY KEY, -- Postgresql
	name VARCHAR(300) NOT NULL
);
```

### Let's insert some dummy data:

```sql
-- INSERT INTO cities (name)
-- VALUES ('Seoul'), ('New York'), ('Tokyo');
  
-- INSERT INTO addresses (street, house_number, city_id)
-- VALUES ('Gui', '202', 1), ('Cedar Glade', '3087', 2), ('teststreet', '8A', 3), ('Gangnam', '123', 4);
  
INSERT INTO users (first_name, last_name, email, address_id)
VALUES ('David Myung Hoon', 'Kim', 'dk@emaill.com', 1), ('David', 'Kim', 'ny@email.com', 2), ('nakamoto', 'yoshida', 'jp@email.com', 3);
```
Inserting the data must be in order since tables depend on another table: `cities` -> `addresses` -> `users`

## Now, let's INNER JOIN our tables:

We can select everything from `users` table and inner join the addresses table.
```sql
SELECT *
FROM users
INNER JOIN addresses ON users.address_id = addresses.id;
```
Reason using the SQL dot notation is that SQL doesn't know which table's `id` field it needs to get the data from. Also, there can be fields with same column name.

We can also give an alias name to the column:
```sql
SELECT *
FROM users AS usr
INNER JOIN addresses AS addr ON usr.address_id = addr.id;
```

We can also pick the fields we want to select:
```sql
SELECT usr.id, first_name, last_name, street, house_number, city_id
FROM users AS usr
INNER JOIN addresses AS addr ON usr.address_id = addr.id
```
This will only grab the specified columns. Notice how it says `usr.id`, this is because we need to be explicit about which table we want `id` field from.

## Multiple JOINs

```sql
SELECT usr.id, first_name, last_name, street, house_number, ci.name AS city_name
FROM users AS usr
INNER JOIN addresses AS addr ON usr.address_id = addr.id
INNER JOIN cities AS ci ON addr.city_id = ci.id;
```

## Filtering

```sql
SELECT usr.id, first_name, last_name, street, house_number, ci.name AS city_name
FROM users AS usr
INNER JOIN addresses AS addr ON usr.address_id = addr.id
INNER JOIN cities AS ci ON addr.city_id = ci.id
WHERE ci.name = 'Seoul';
```
This will filter for records with `Seoul`.

### Ordering

```sql
SELECT usr.id, first_name, last_name, street, house_number, ci.name AS city_name
FROM users AS usr
INNER JOIN addresses AS addr ON usr.address_id = addr.id
INNER JOIN cities AS ci ON addr.city_id = ci.id
WHERE ci.name = 'Seoul' OR ci.id = 2
ORDER BY usr.id DESC;
```
