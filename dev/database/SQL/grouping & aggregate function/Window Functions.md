
[MySQL Window Function Description](https://dev.mysql.com/doc/refman/8.0/en/window-function-descriptions.html)

A bit like aggregate functions. But, with window functions, you can apply aggregate functions or other function types but the results will be added in a new column without reducing the actual input values of the underlying table to a single row/value.

```sql
SELECT booking_date, amount_tipped
FROM bookings;

SELECT SUM(amount_tipped) FROM bookings;
```
With this basic query, we select all the `booking_date` and `amount_tipped` from the `bookings` table. And we can also get the sum of all `amount_tipped` as well.

Now, if we want to show the sum of `amount_tipped` in each row by adding a column:
```sql
SELECT booking_date, amount_tipped, SUM(amount_tipped) OVER()
FROM bookings;
```
We use the `OVER()` key.

| booking_date | amount_tipped | sum    |
| ------------ | ------------- | ------ |
| 2023-11-04   | 0.10          | 119.10 |
| 2023-11-04   | 0.00          | 119.10 |
| 2023-11-05   | 12.10         | 119.10 |
| ...          | ...           | 119.10 |

If we want to group each `booking_date` and calculate the sum of the grouped `booking_date`:
```sql
SELECT booking_date, amount_tipped, SUM(amount_tipped) OVER(PARTITION BY booking_date)
FROM bookings;
```
We use the `OVER()` key and give a `PARTITION` keyword by saying we want to group by `booking_date`.

| booking_date | amount_tipped | sum    |
| ------------ | ------------- | ------ |
| 2023-11-04   | 0.10          | 0.10   |
| 2023-11-04   | 0.00          | 0.10   |
| 2023-11-05   | 12.10         | 106.40 |
| 2023-11-05   | 5.30          | 106.40 |
| 2023-11-05   | 89.00         | 106.40 |
| ...          | ...           | ...    |

## With ORDER BY

```sql
SELECT booking_date, amount_tipped, amount_billed, SUM(amount_tipped)
OVER(PARTITION BY booking_date ORDER BY amount_billed DESC)
FROM bookings;
```
We can use `ORDER BY` in our window function that orders in descending orders (in our case).
The result will look something like:

| booking_date | amount_tipped | amount_billed | sum   |
| ------------ | ------------- | ------------- | ----- |
| 2023-11-04   | 0.10          | 19.90         | 0.10  |
| 2023-11-04   | 0.00          | 12.90         | 0.10  |
| 2023-11-05   | 10.10         | 140.90        | 10.10 |
| 2023-11-05   | 6.60          | 113.40        | 16.70 |
| 2023-11-05   | 1.40          | 98.60         | 18.10 |
| ...          | ...           | ...           | ...   |
Notice that `sum` calculation applies incrementally. Also, we have ordered our list ordered by `amount_billed` in descending order.

## RANK

We can rank the `amount_tipped` in order as well.

```sql
SELECT booking_date, amount_tipped, RANK()
OVER(PARTITION BY booking_date ORDER BY amount_tipped DESC NULLS LAST)
FROM bookings;
```
It would rank the `amount_tipped` values in descending order.
💥 There's seems to be an issue where in descending order, `NULL` gets sorted to the top [issue](https://stackoverflow.com/questions/7621205/sort-null-values-to-the-end-of-a-table). This can be solved with `NULLS LAST` for PostgreSQL 8.3 or up.

| booking_date | amount_tipped | rank |
| ------------ | ------------- | ---- |
| 2023-11-04   | 0.10          | 1    |
| 2023-11-04   | 0.00          | 2    |
| 2023-11-05   | 10.10         | 1    |
| 2023-11-05   | 6.60          | 2    |
| 2023-11-05   | 1.40          | 3    |
| 2023-11-05   | NULL          | 4    |
| ...          | ...           | ...  |
