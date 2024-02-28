
Let's just create a database and table:
```sql
CREATE DATABASE sales_example;

CREATE TABLE sales (
	-- id INT PRIMARY KEY AUTO_INCREMENT, -- MySQL
	id SERIAL PRIMARY KEY, -- Postgres
	date_created DATE DEFAULT (CURRENT_DATE),
	date_fulfilled DATE,
	customer_name VARCHAR(300) NOT NULL,
	product_name VARCHAR(300) NOT NULL,
	volume NUMERIC(10, 3) NOT NULL CHECK (volume >= 0),
	is_recurring BOOLEAN DEFAULT FALSE,
	is_disputed BOOLEAN DEFAULT FALSe
);
```

## Insert (create)

We can create a record in our table:
```sql
-- Single insert
INSERT INTO sales (customer_name, product_name, volume, is_recurring)
VALUES ('DK', 'Iphone 15', 999.99, TRUE);

-- Multiple insert
INSERT INTO sales (
date_fulfilled,
customer_name,
product_name,
volume,
is_recurring,
is_disputed
)
VALUES (
	NULL,
	'Toyota',
	'Supra',
	4889.23,
	FALSE,
	FALSE
),
(
	'2024-02-27',
	'Honda',
	'S2000',
	3000.88,
	FALSE,
	TRUE
);
```

## Update

Let's say we want to update a record from `sales` table.
Assume that the `id` is 13 and we want to update `product_name` and `volume`.

```sql
UPDATE sales
SET
	product_name = 'A Truck'
	volume = volume * 1000
WHERE id = 13;
```
We are updating the record of `product_name` to be 'A Truck' and `volume` to be whatever it is stored in the database multiply by 1000.

## Delete

There are times when we need to delete a record.
It is very simple to just delete a row/record from the table:

```sql
DELETE FROM sales
WHERE id = 13;
```
Of course, we can add logics into it.

## Select

Select everything from sales table:
```sql
SELECT * FROM sales;
```

Select certain columns from the table:
```sql
SELECT customer_name, product_name, volume, date_created FROM sales;
```
The order of the columns will depend on the order it is called. In our case, the data will be in the order of our SELECT command.

We can also transform the key when getting the data or event give them a dummy value.

```sql
SELECT
	customer_name,
	product_name,
	volume AS total_volume,
	date_created
FROM sales;
```
This will rename the `volume` column as `total_volume`. But it will NOT affect the database.

```sql
SELECT
	'DK',
	product_name,
	volume / 1000 AS total_sales,
	date_created
FROM sales;
```
This divides the `volume` data by 1000 and renames to `total_sales`. It also just puts 'DK' as customer_name instead of showing the actual data.



