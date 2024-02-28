
Let's take a look at how to do basic SQL commands with MySQL and PostgreSQL. 

- Create a database called `online_shop`.
- Create a table called `products` with for product name, price of the product, description of the product, amount in stock, path for product image.
- Insert dummy data into the table.
- Update the table with constraints.
- Add `id` column as a primary key

```sql
-- Task 1: Create database
CREATE DATABASE online_shop;

-- Task 2: Create table and configure for products
CREATE TABLE products (
name VARCHAR(200),
price NUMERIC(10, 2),
description TEXT,
amount_in_stock SMALLINT,
image_path VARCHAR(500)
);

-- Task 4: Inserting dummy data
INSERT INTO products (name, price, description, amount_in_stock, image_path)
VALUES ('Harry Potter', 5.99, 'Prisoner of dkwmzkqks', 3, 'uploads/images/products/harry_potter.jpg');

-- Taks 5: Update table and add sensible constraints
ALTER TABLE products
-- MySQL
MODIFY COLUMN name VARCHAR(200) NOT NULL,
MODIFY COLUMN price INT NOT NULL CHECK (price > 0),
MODIFY COLUMN description TEXT NOT NULL,
MODIFY COLUMN amount_in_stock SMALLINT UNSIGNED;
-- PostgreSQL
ALTER COLUMN name SET NOT NULL,
ALTER COLUMN price SET NOT NULL,
ALTER COLUMN description SET NOT NULL,
ADD CONSTRAINT price_positive CHECK (price > 0),
ADD CONSTRAINT amount_in_stock CHECK (amount_in_stock >= 0)

-- Task 6: Add id column
-- MySQL
ALTER TABLE products
ADD COLUMN id INT PRIMARY KEY AUTO_INCREMENT;
-- PostgreSQL
ADD COLUMN id SERIAL PRIMARY KEY;
```
