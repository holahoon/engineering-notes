
We can also have self-referencing many-to-many relationships.
Take an example of a social network app. Here, we can have many users and these users can be friends with other users.

### Create table

```sql
CREATE TABLE users (
	id SERIAL PRIMARY KEY,
	first_name VARCHAR(300) NOT NULL,
	last_name VARCHAR(300) NOT NULL
	-- ...
);

-- intermediate table
CREATE TABLE users_friends (
	user_id INT REFERENCES users (id) ON DELETE CASCADE,
	friend_id INT REFERENCES users (id) ON DELETE CASCADE,
	CHECK (user_id < friend_id), -- prevents from having same relationships in two different rows
	PRIMARY KEY (user_id, friend_id)
);
```

So, we create `users_friends` intermediate table. In this table, we self-reference the `users` table. Notice that we have the `CHECK` constraint where it checks if `user_id` is less than `friend_id`. This is needed because we need to prevent from having same relationships in two different row (in our case). If not, we can end up having `1 & 2` and `2 & 1` which is the same expression.

### Insert data

```sql
INSERT INTO users (first_name, last_name)
VALUES ('David', 'Kim'), ('Sohyeon', 'Kim'), ('Jinho', 'Kim');
```

```sql
INSERT INTO users_friends (user_id, friend_id)
VALUES (1, 2), (1, 3);
```