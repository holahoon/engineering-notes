The WHERE clause is useful when you want to filter data in a table based on a given condition in the SQL statement.The WHERE clause in SQL is there for the purpose of filtering records and fetching only the necessary records. This can be used in SQL SELECT, UPDATE and DELETE statements.

The filtering happens based on a condition. The condition can be written using any of the following comparison or logical operators.

## Comparison Operators

|**Operator**|**Description**|
|---|---|
|=|Checks if the values of two operands are equal or not. If yes, then condition becomes true.|
|!=|Checks if the values of two operands are equal or not. If values are not equal, then condition becomes true.|
|<>|Checks if the values of two operands are equal or not. If values are not equal, then condition becomes true.|
|>|Checks if the value of the left operand is greater than the value of the right operand. If yes, then condition becomes true.|
|<|Checks if the value of left operand is less than the value of right operand. If yes, then condition becomes true.|
|>=|Checks if the value of the left operand is greater than or equal to the value of right operand. If yes, then condition becomes true.|
|<=|Check if the value of the left operand is less than or equal to the value of the right operand. If yes then condition becomes true.|
|!<|Checks if the value of the left operand is not less than the value of the right operand. If yes, then condition becomes true.|
|!>|Checks if the value of the left operand is not greater than the value of the right operand. If yes, then condition becomes true.|

## Logical Operators

|**Operator**|**Description**|
|---|---|
|**ALL**|Used to compare a single value to all the values in another value set.|
|**AND**|Allows for the existence of multiple conditions in an SQL statement's WHERE clause.|
|**ANY**|Used to compare a value to any applicable value in the list as per the condition.|
|**BETWEEN**|Used to search for values that are within a set of values, given the minimum value and the maximum value.|
|**EXISTS**|Used to search for the presence of a row in a specified table that meets a certain criterion.|
|**IN**|Used to compare a value to a list of literal values that have been specified.|
|**LIKE**|Used to compare a value to similar values using wildcard operators.|
|**NOT**|Reverses the meaning of the logical operator with which it is used. For example: NOT EXISTS, NOT BETWEEN, NOT IN, etc. **This is a negate operator.**|
|**OR**|Used to combine multiple conditions in an SQL statement's WHERE clause.|
|**IS NULL**|Used to compare a value with a NULL value.|
|**UNIQUE**|Searches every row of a specified table for uniqueness (no duplicates).|

Let's review an example that uses the comparison operator > (greater than) to formulate the WHERE clause condition to filter criteria. If you want to fetch the invoices that have a total value of more than $2, you will need to filter out the records in the `invoice` table by using the WHERE clause in the SELECT statement:
```sql
SELECT *
FROM invoices
WHERE Total > 2;
```

This query filters out the records based on the condition given in the WHERE clause `Total` > 2. It brings in only the records that have a `Total` field value of more than $2.

But what if you want to combine multiple conditions in the WHERE clause? Multiple conditions in the WHERE clause can be combined using the AND / OR logical operators. Therefore, these two operators are also known as conjunctive operators:
```sql
SELECT column1, column2, columnN
FROM table_name
WHERE [condition1] AND [condition2]...AND [conditionN];
```

For example, you need a list of invoices for which the total is over $2 and the BillingCountry is the USA. Here’s an example of how the WHERE clause condition can be given in the SELECT statement:
```sql
SELECT *
FROM invoices
WHERE Total > 2 AND BillingCountry = 'USA';
```
Here, the AND operator is used as a conjunctive operator to combine the two conditions Total > 2 AND BillingCountry which is the USA. You'll receive the invoice records with a total bill value of more than $2 with the USA as billing country. This means that for a record to be included in the result, both the conditions should be true.

Similarly, the OR operator can also be used to combine multiple conditions in the WHERE clause:
```sql
SELECT column1, column2, columnN
FROM table_name
WHERE [condition1] OR [condition2]...OR [conditionN]
```

If you want to get a list of invoices for which the BillingCountry is the USA or France, how would you use the OR operator to combine the two conditions?
```sql
SELECT *
FROM invoices
WHERE BillingCountry = 'USA' OR BillingCountry='France';
```
You’ll notice that the result consists of records where the billing country is the USA or France. This means that for a record to be included in the result, either condition should be true.

Let’s consider another scenario. If you want to get a list of invoices where the total value is over $2 and the BillingCountry is USA or France, here’s the syntax for the SELECT query using both AND / OR conjunctive operators together in the WHERE clause:
```sql
SELECT *
FROM invoices
WHERE Total > 2 AND (BillingCountry = 'USA' OR BillingCountry = 'France');
```
