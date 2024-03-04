
[MySQL](https://dev.mysql.com/doc/refman/8.0/en/date-and-time-functions.html)
[PostgreSQL](https://www.postgresql.org/docs/current/functions-datetime.html)

Assume we have a timestamp in our `memberships` table. The format of the timestamp is `YYYY-MM-DD hh:mm:ss`

## Calendar day

We can use `MONTH`, `DAY`, ...
```sql
SELECT EXTRACT(MONTH FROM last_checkin) AS month, EXTRACT(DAY FROM last_checkin) AS day, last_checkin
FROM memberships;
```

| month | day | last_checkin        |
| ----- | --- | ------------------- |
| 3     | 24  | 2024-03-24 05:17:36 |
| 12    | 12  | 2023-12-12 11:20:59 |
| ...   | ... | ...                 |

## Weekday

This is specific for PostgreSQL.
Using `DOW` will get us the weekday value:
```sql
SELECT EXTRACT(DOW FROM last_checkin) AS month, last_checkin
FROM memberships;
```
0 is Sunday, and 6 is Saturday.
If we use `ISODOW`, Sunday will be 7.
```sql
SELECT EXTRACT(ISODOW FROM last_checkin) AS month, last_checkin
FROM memberships;
```

### MySQL

MySQL has a different syntax:
```sql
SELECT WEEKDAY(last_checkin), last_checkin
FROM memberships;
```
However the count of the weekday function starts with 0 as Monday and 6 as Sunday.
Of course, we can add 1 to the result which can match the `ISODOW` in PostgreSQL.
```sql
SELECT WEEKDAY(last_checkin) + 1, last_checkin
FROM memberships;
```

## Convert

This is specific for MySQL:
```sql
SELECT CONVERT(last_checkin, DATE), CONVERT(last_checkin, TIME)
FROM memberships;
```
We can convert certain field to be in a specific format. This will return something like:

| last_checkin | last_checkin(1) |
| ------------ | --------------- |
| 2024-03-24   | 05:17:36        |
| 2023-12-12   | 11:20:59        |
| ...          | ...             |

### PostgreSQL
```sql
SELECT last_checkin::TIMESTAMP::DATE, last_checkin::TIMESTAMP::TIME
FROM memberships;
```
