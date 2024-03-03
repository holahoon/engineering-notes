
We can apply conditional filters.
## Filter

```sql
SELECT ROUND(AVG(amount_tipped), 2) FROM bookings
WHERE amount_billed > 20 AND num_guests > 2;
```
We grab the average of the `amount_tipped` column and rounded it up with 2 decimal points where the `amount_billed` value is greater than 20 and `num_guests` are more than 2.
## Join

```sql
SELECT MAX(num_guests) AS num_guests, MAX(num_seats) AS num_seats
FROM bookings AS b
INNER JOIN tables AS t ON b.table_id = t.id;
```
We join `bookings` and `tables` table and grab the maximum value of `num_guest` column in `bookings` table and maximum value of `num_seats` column in `tables` table.
## Join & Filter

```sql
SELECT MAX(num_guests) AS num_guests, MAX(num_seats) AS num_seats
FROM bookings AS b
INNER JOIN tables AS t ON b.table_id = t.id
INNER JOIN payment_methods AS pm ON b.payment_id = pm.id
WHERE t.num_seats < 5 AND pm.name = 'Cash'
```
We join `bookings`, `tables`, `payment_methods` tables and filter by looking for `num_seats` in `tables` table are greater 5 and `name` column in `payment_methods` table is `'Cash'`.