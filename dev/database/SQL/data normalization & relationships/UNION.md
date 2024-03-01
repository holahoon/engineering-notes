
UNION is a clause that combines multiple result sets into one result set by appending rows, whereas JOIN clauses merge multiple tables into one result set by appending columns.

```sql
SELECT * FROM users
WHERE id < 3
UNION
SELECT * FROM users
WHERE id > 5;
```
(Of course this is just an example. We can def use `OR`).

```sql
SELECT id, first_name FROM users
UNION
SELECT id, street FROM addresses;
```
`id` and `street` from `addresses` get appended to `users`.

⚠️ You need to ensure that you have the same number of columns. It can't add rows if results have different row quantities.
```sql
SELECT * FROM users
UNION
SELECT ** FROM addresses;
```
This will result an error saying something like "The used SELECT statements have a different number of columns".
## Use cases

Typically `UNION` is used to append rows with similar data to another result set.