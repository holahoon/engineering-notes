
There are always times when we can have duplicated records in a table.
We can run to only get unique and distinctive values:

```sql
SELECT DISTINCT customer_name FROM sales;
```
This will only query `customer_name` and give only unique names(non-duplicated names).