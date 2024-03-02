
`Aggregate` is a mathematical operation returning a single (aggregated) result. `Function` is a predefined operation, which can be executed on demand.
For example,
```sql
SELECT SUM(salary) FROM employeed;
```
is an aggregate function which returns a value.

## Core Aggregate Functions Overview

`COUNT()`, `SUM()`, `MAX()`, `MIN()`, `AVERAGE()`

[MySQL aggregate function](https://dev.mysql.com/doc/refman/8.0/en/aggregate-functions.html)
[PostgreSQL aggregate functions](https://www.postgresql.org/docs/current/functions-aggregate.html)

## Count()

```sql
SELECT COUNT(*) FROM bookings;
```
We count all the rows and get the total length from `bookings` table.

```sql
SELECT COUNT(amount_tipped) FROM bookings;
```
We count only the `amount_tipped` column. If a value is `NULL`, it doesn't count.

```sql
SELECT COUNT (DISTINCT booking_date) FROM bookings;
```
We count the distinct values in `booking_date` column.

## Min(), Max()

```sql
SELECT MAX(num_seats) FROM tables;
```
We grab the maximum value in `num_seats` column from the `tables` table.

```sql
SELECT MIN(num_seats) FROM tables;
```
We grab the minimum value.

```sql
SELECT MIN(*) FROM tables;
```
⚠️ Will NOT work.

```sql
SELECT MAX(amount_billed) AS max_billed, MAX(amount_tipped) AS max_tipped FROM bookings;
```
Grabs the maximum value in `amount_billed` column, and maximum value in `amount_tipped` column from `bookings` table and gives them an alias name.
We can also use it with string values, date values with for `MIN()` and `MAX()`. Obviously, minimum will grab the smaller value.

## SUM(), AVG(), ROUND()

```sql
SELECT SUM(amount_billed) FROM bookings;
```
Calculates the sum of all the rows on `amount_billed` column in `bookings` table.

```sql
SELECT AVG(num_guests) FROM bookings;
```
Grabs the average of `num_guests` column.
✏️ `AVG()` ignores `NULL` values.

```sql
SELECT ROUND(AVG(num_guests), 2) FROM bookings;
```
We round up the value we got by grabbing the average of `num_guests`. `2` is used to describe the decimal points.
