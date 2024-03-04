
## LIKE

`LIKE` always returns TRUE/FALSE boolean value.
It is also used to match exact value.

```sql
SELECT first_name LIKE 'David'
FROM memberships;
```
Returns `TRUE` if matched. MySQL would return `1` or `0`

## Pattern Matching

We can add `%` sign to the value we want to look for. This tells that I want to look for a value that has `'Da'` and the rest is unknown.

```sql
SELECT first_name LIKE 'Da%'
FROM memberships;
```
This would return `TRUE` if a value contains `'Da'`.

If we don't know the first letter, but we know that we want to look for a string that contains `'a'` in the second position, we can use `_` in front of it:
```sql
SELECT first_name LIKE '_a%'
FROM memberships;
```

```sql
SELECT first_name
FROM memberships
WHERE first_name LIKE 'So_____';
```
This would only match a string like `'So` with 5 other letters after it.

### Case Sensitive

`LIKE` is also case-sensitive. We can add `I` in front of `LIKE` -> `ILIKE`
```sql
SELECT first_name ILIKE 'd%', first_name
FROM memberships;
```
This would match any string values with either `'D'` or `'d'` in front of the letter.
Of course, we can just use `AND` to check for both lowercase and uppercase.