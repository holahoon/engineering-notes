
## WHERE

```sql
SELECT booking_date, COUNT(booking_date)
FROM bookings
WHERE amount_billed > 30
GROUP BY booing_date;
```
This is just a normal filter based on single booking.
Before `GROUP BY`, filtering takes place which therefore `amount_billed` will scan each value to see if larger than 30 or not.

- `WHERE` is applied before the `GROUP BY` (before any aggregation).
- Filter is applied to the "raw data" (un-aggregated data).
- Aggregate functions are NOT allowed as filter conditions.
- Whenever you don't use aggregate function, you should use `WHERE`

## HAVING

```sql
SELECT booking_date, COUNT(booking_date)
FROM bookings
GROUP BY booking_date
HAVING SUM(amount_billed) > 30;
```
`HAVING` is positioned after the `GROUP BY`, therefore it initiates after `GROUP BY`.
We filter based on each `booking_date` then `HAVING` works after `GROUP BY`.

- `HAVING` is a filter to use after aggregated values with the `GROUP BY` keyword.
- Filter is applied to the aggregated data (with `GROUP BY`).
- Aggregate functions are allowed as filter conditions.
- Whenever you need to use aggregate function, use `HAVING`
