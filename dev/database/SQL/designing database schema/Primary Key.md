A relational database is a collection of data that is managed and maintained in a database management system such as Oracle or MySQL. A relational database enables you to retrieve every single piece of stored data. This can be done by specifying:
- the name of the target table (or tables),
- the name of the required column (or columns), 
- and the primary key of the table. 

In a relational database, the primary key can be selected from any candidate key attribute that contains a unique instance value in each row of the table.

## Example

Let's create a database and use it:
```sql
CREATE DATABASE automobile;
USE automobile;
```

Let's create a table with columns `vehicle_id`, `owner_id`, `plate_number`, `phone_number` and primary set to the `vehicle_id`:
```sql
CREATE TABLE vehicle(vehicle_id VARCHAR(10), owner_id VARCHAR(10), plate_number VARCHAR(10), phone_number INT PRIMARY KEY(vehicle_id));
```
Any candidate key that has not been chosen to act as the primary key of the vehicle table is called an alternate key. That’s the plate number and phone number.

Also, it’s important to remember that a primary key can be composed of one single simple attribute or multiple attributes (a composite key), which is composed of two or more attributes that form a unique value in each row of the table. This usually happens when a single attribute cannot be found to act as a primary key.

We can see that the table has been create and view the columns within the `vehicle` table:
```sql
SHOW tables;
SHOW columns for vehicle;
```

Let's build another table called `owner` which is about the vehicles' owners.
```sql
CREATE TABLE owner(owner_id VARCHAR(10), owner_name VARCHAR(50), owner_address VARCHAR(255), PRIMARY KEY(owner_id));
```
This will create a table with `owner_id` set as primary key.

You may have noticed that the owner ID is a common attribute, as it exists in both tables. However, the key difference is that it is a primary key in the owner table. This means it must be unique in each row of the table. Yet it might be duplicated in the vehicle table, because the same owner might have multiple vehicles. This means that the owner ID in the vehicle table is a suitable choice for a foreign key that connects the vehicle table with the owner table as shown in the following ER diagram.
![[Pasted image 20240215104117.png]]

To create this relationship in the actual database you need to modify the vehicle table structure to make the owner ID a foreign key. This can be done by using the “ALTER” command to change the structure of the vehicle table. You can also use the "ADD” command to define the owner ID as the foreign key. Finally, you can use a "REFERENCES” keyword to reference it with the primary key in the owner table.

```sql
ALTER TABLE vehicle ADD FOREIGN KEY(owner_id) REFERENCES owner(owner_id);
```

You will notice that the owner ID key has been changed into a MUL key, which is one of the three possible values for the "key" attribute in MySQL.
- `PRI` comes from primary; this means it’s a primary key. 
- `UNI` comes from unique; this means it’s a unique key. 
- `MUL` comes from multiple. If the key is MUL, it means that the related column is permitted to contain the same value in multiple cells of that column.
