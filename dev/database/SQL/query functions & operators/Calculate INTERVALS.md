
[PostgreSQL](https://www.postgresql.org/docs/9.5/functions-datetime.html)
[MySQL](https://dev.mysql.com/doc/refman/8.0/en/date-and-time-functions.html)

Intervals measure the time elapsed between two points in time.

```sql
SELECT last_checkout - last_checkin fROM memberships;
```
In PostgreSQL, it prints something like `{"hours": 1, "minutes": 3, "seconds": 9}`.

In MySQL, it prints somethings like `10309`.
So, in MySQL, we can do something like:
```sql
SELECT TIMESTAMPDIFF(MINUTE, last_checkin, last_checkout)
FROM memberships;
```
Here, we would get like `63`, `268` in minutes.

## Intervals in Date

```sql
SELECT membership_end - membership_start
FROM memberships;

-- MySQL
-- SELECT DATEDIFF(membership_end, membership_start)
-- FROM memberships;
```
This will give us values in days. So something like: `729`, `31`, ... days.

We can also get intervals from now.
```sql
SELECT NOW() - membership_start
FROM memberships;

-- MySQL
-- SELECT DATEDIFF(NOW(), membership_start)
-- FROM memberships;
```
In PostgreSQL, it prints something like `{"days": 35, "hours": 20, "minutes": 13, "seconds": 9}`.

## Adding INTERVALS to Dates

```sql
SELECT membership_start + 7, membership_start
FROM memberships;

-- MySQL
-- SELECT DATE_ADD(membership_start, INTERVAL 7 DAY), membership_start
-- FROM memberships;
```
This adds extra 7 days to the `membership_start` column.
We can use `MONTH` or `YEAR` keyword instead as well in MySQL.

What about for PostgreSQL?
```sql
SELECT (membership_start + INTERVAL '7 MONTHS')::TIMESTAMP::DATE, membership_start
FROM memberships;
```
With PostgreSQL, without the `::TIMESTAMP::DATE` suffix logic, we would give us in format like `YYYY-MM-DD hh:mm:ss`. We can use this suffix logic to only get the `YYYY-MM-DD`.
Also, for Postgres, we can use plural(`MONTHS`) or singular(`MONTH`).