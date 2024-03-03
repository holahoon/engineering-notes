
A query that builds on a result of a different query.

## Normal aggregated query

```sql
SELECT booking_date, SUM(amount_billed) AS daily_sum
FROM bookings
GROUP BY booking_date;
```
We select the entire `bookings` table by grouping `booking_date` and get the sum of `amount_billed`.

| booking_date | sum    |
| ------------ | ------ |
| 2023-11-05   | 428.90 |
| 2023-11-08   | 38.60  |
| 2023-11-06   | 669.10 |
| 2023-11-04   | 32.80  |
| 2023-11-05   | 293.90 |

## Nested Subqueries

```sql
SELECT MIN(daily_sum)
FROM (
	SELECT booking_date, SUM(amount_billed) AS daily_sum
	FROM bookings
	GROUP BY booking_date
) AS daily_table;
```
We get the minimum value of all the sum that we got from the aggregated query.
✏️ Notice that we give the query a "table" name as `daily_table`.

| sum   |
| ----- |
| 32.80 |

## More Nested Subqueries

```sql
SELECT booking_date
FROM bookings
GROUP BY booking_date
HAVING SUM(amount_billed) = (
	SELECT MIN(daily_sum)
	FROM (
		SELECT booking_date, SUM(amount_billed) AS daily_sum
		FROM bookings
		GROUP BY booking_date
	) AS daily_table
);
```
We can go even further by selecting the `booking_date` by looking the sum of `amount_billed` is equal to the minimum sum value we got above.
Of course, the subqueries can be applied to the `WHERE` filter as well.