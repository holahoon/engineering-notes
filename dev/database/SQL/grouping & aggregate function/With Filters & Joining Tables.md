
We can apply conditional filters.

```sql
SELECT ROUND(AVG(amount_tipped), 2) FROM bookings
WHERE amount_billed > 20 AND num_guests > 2;
```
We grab the average of the `amount_tipped` column and rounded it up with 2 decimal points where the `amount_billed` value is greater than 20 and `num_guests` are more than 2.







