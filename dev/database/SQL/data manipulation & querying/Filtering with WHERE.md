
## Available Comparison Operations & Variations
### =, IS...

- Check for equality
- `=` for most cases
- `IS...` for comparison to `NULL` (and others)
### <>, !=, is NOT...

- Check for inequality
- `!=` is an alias for `<>`
- `IS NOT...`for comparison to `NULL` (and others)
### > / >=

- Check for greater than / greater than or equal
### < / <=

- Check for smaller than / smaller than or equal
### AND , OR

- Combine multiple conditions

## Practice

### Look for everything in `sales` table where volume is greater than 1000:

```sql
SELECT * FROM sales
WHERE volume > 1000;
```

### Select where `is_recurring` is `TRUE`:

```sql
SELECT * FROM sales
WHERE is_recurring IS TRUE;
-- WHERE is_recurring = 0; -- is equal to 0 which means it is FALSE
-- WHERE is_recurring <> 0; -- not equal to 0 which means it is TRUE
```
✏️ TRUE/FALSE in database are basically just 1 / 0.

### Look for where `is_disputed` is `TRUE` and  `volume` is greater  than 2000:

```sql
SELECT * FROM sales
WHERE (is_disputed IS TRUE) AND (volume > 2000);
```
✏️ `()` is used just to make it easier to read

### Look for date ranges:

```sql
-- SELECT * FROM sales
-- WHERE date_created > '2021-01-01' AND date_created < '2022-05-01';

SELECT * FROM sales
WHERE date_created BETWEEN '2021-01-01' AND '2022-05-01';
```
✏️ The dates will be excluded for the range.

### Working with date differences:

Get the date difference between `date_fulfilled` and `date_created` that is less than 5 days:
```sql
SELECT * FROM sales
WHERE date_fulfilled - date_created <= 5;
```

There can be times when we store the data type as `TIMESTAMP` instead of `DATE`. If we just subtract two different days, we can get the result in days, but dealing with timestamp is different.
We can utilize the built-in function `EXTRACT` to extract out the day from the calcuation:
```sql
SELECT * FROM sales
WHERE EXTRACT(DAY FROM date_fulfilled - date_created) <= 5;
```


