
Also called "LEFT OUTER JOIN".
The Left table(the table select from) will have all of its rows included(unless filtered), and the Right table will only have the matching records.

Just as with INNER JOIN, LEFT JOIN does not just consider the "left table" but actually the entire result set that was derived up to the LEFT JOIN clause.

## [[INNER JOIN#Let's create a sample database]]

Any table select after the `FROM` keyword is the left table.
```sql
SELECT *
FROM users AS u
LEFT JOIN addresses AS a ON a.id = u.address_id;
```
So, left table would be `users` and everything in `users` table will in selected with the matching `addresses` records.

If we flip it:
```sql
SELECT *
FROM addresses AS a
LEFT JOIN users AS u ON a.id = u.address_id;
```
The left table is `addresses` and it all the records in this table will be selected, but only the matching `users` records will be joined. With our current dummy database, it can see that we can 4 rows of `addresses` but one of the row will have a bunch of `NULL` values since there's no matching record in the `users` table.

## Multiple LEFT JOINs

```sql
SELECT *
FROM addresses AS a
LEFT JOIN users AS u ON a.id = u.address_id
LEFT JOIN cities AS c ON c.id = a.city_id;
```
Keep in mind that the second LEFT JOIN is based on the initial LEFT JOIN results.

## What about RIGHT JOINs?

```sql
SELECT *
FROM users AS u
RIGHT JOIN addresses AS a ON a.id = u.address_id;
```

This is the same as 
```sql
SELECT *
FROM addresses AS a
LEFT JOIN users AS u ON a.id = u.address_id;
```