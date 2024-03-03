
We learned that we can work with two aggregate functions as identifiers:
```sql
SELECT COUNT(booking_date) as nr_bookings, SUM(num_guests) as nr_guests FROM bookings;
```
Here, `COUNT(booking_date)` and `SUM(num_guests` functions are aggregate functions that calculate based on some specific field.
What if we have aggregated and non-aggregated identifiers??

```sql
SELECT booking_date AS date, SUM(num_guests) AS nr_guests FROM bookings;
```
We grab all a list of `booking_date` column. But for our aggregate function, we don't have a base for a calculation.

We can solve this issue by using `GROUP BY` keyword and tell SQL in which column duplicate values should be grouped in a single value.
```sql
SELECT booking_date AS date, SUM(num_guests) AS nr_guests
FROM bookings
GROUP BY booking_date;
```
With this, we have groups of each individual `booking_date` without duplicate values. Now aggregation can take place. Based on this grouping, group takes place before aggregation, the aggregation function knows that we want to calculate the sum of the number of guests for each individual `booking_date`.

👏 In summary, we have to ensure that the base for the aggregation is clear.

## Example

### Basic GROUP BY

```sql
SELECT booking_date AS booking_date, SUM(num_guests) AS num_guests
FROM bookings
GROUP BY booking_date;
```

We get something like this:

| booking_date | num_guests |
| ------------ | ---------- |
| 2023-11-05   | 26         |
| 2023-11-06   | 4          |
| 2023-11-07   | 10         |

Keep in mind that we can't use `DISTINCT` keyword for using aggregate function with non-aggregate function
💥 This will **NOT** work
```sql
SELECT DISTINCT booking_date AS booking_date, SUM(num_guests) AS num_guests
FROM bookings;
```

### With Joined Queries

```sql
SELECT pm.name AS name, SUM(b.num_guests) AS num_guests
FROM payment_methods AS pm
INNER JOIN bookings AS b ON b.payment_id = pm.id
GROUP BY pm.name;
```

| name        | num_guests |
| ----------- | ---------- |
| Credit Card | 24         |
| Cash        | 56         |

We can take this even further:
```sql
SELECT b.booking_date AS date, pm.name AS payment_method, SUM(b.num_guests) AS num_guests, ROUND(AVG(b.amount_tipped), 2) AS tips
FROM bookings AS b
INNER JOIN payment_methods AS pm On pm.id = b.payment_id
GROUP BY b.booking_date, pm.name
ORDER BY b.booking_date DESC;
```

| date       | payment_method | num_guests | tips  |
| ---------- | -------------- | ---------- | ----- |
| 2023-11-08 | Credit Card    | 2          | 0.80  |
| 2023-11-07 | Credit Card    | 11         | 10.00 |
| 2023-11-07 | Cash           | 4          | 3.05  |
| 2023-11-06 | Credit Card    | 19         | 11.68 |
| 2023-11-06 | Cash           | 11         | 7.67  |
