## What is database structure?

Database structure refers to how data is arranged in a database. Within a database, related data are grouped into tables, each of which consists of rows (also called tuples) and columns, like in a spreadsheet.

The structure of a database consists of a set of key components. These include:
- **Tables or entities**, where the data is stored. 
- **Attributes** which are details about the table or entity. In other words, attributes describe the table. 
- **Fields**, which are columns used to capture attributes. 
- A **record**, which is one row of details about a table or entity. 
- And the **primary key**, which is a unique value for an entity. 

This image shows the basic structural elements of a database table.

![[Pasted image 20240211201932.png]]

## Logical database structure

The logical structure of a database is represented using a diagram known as the Entity Relationship Diagram (ERD). It is a visual representation of how the database will be implemented into tables during physical database design, using a Database Management System (DBMS) like MySQL or Oracle, for example. 

A part of the logical database structure is how relationships are established between entities. These relationships are established between the instances of the entities. Accordingly, there can be three ways in which entity instances can be related to each other:
- One-to-one relationships 
- One-to-many relationships 
- Many-to-many relationships 

This is also known as cardinality of relationships. The logical database structure which is represented using an ERD also depicts these relationships.

Here’s an example of an ERD that has all these elements.

![[Pasted image 20240211202244.png]]

## Physical database structure

In the physical database structure, where entities are implemented as tables, the relationships are established using a field known as a foreign key. A foreign key is a field in one table that refers to a common field in another table (usually the primary key).

Let’s take the example of a database that contains two tables: student and department. The student table has a primary key of `Stud_id`, which is also present in the Department table as a foreign key. Therefore, the two tables are related to each other via the `Stud_id` field.

![[Pasted image 20240211202324.png]]
