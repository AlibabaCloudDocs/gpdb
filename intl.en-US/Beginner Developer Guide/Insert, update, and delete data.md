# Insert, update, and delete data

This topic describes how to insert, update, and delete data in AnalyticDB for PostgreSQL.

## Insert data

You can execute the `INSERT` statement to insert one or more rows into a table. Syntax:

```
INSERT INTO table [( column [, ...] )]
   {DEFAULT VALUES | VALUES ( {expression | DEFAULT} [, ...] ) 
   [, ...] | query}
```

For more information, visit [INSERT](http://gpdb.docs.pivotal.io/6-14/ref_guide/sql_commands/INSERT.html).

**Examples:**

Execute the following statement to insert a row into a table:

```
INSERT INTO products (name, price, product_no) VALUES ('Cheese', 9.99, 1);
```

Execute the following statement to insert multiple rows into a table:

```
INSERT INTO products (product_no, name, price) VALUES
    (1, 'Cheese', 9.99),
    (2, 'Bread', 1.99),
    (3, 'Milk', 2.99);
```

Execute the following statement to insert data into a table by using a scalar expression:

```
INSERT INTO films SELECT * FROM tmp_films WHERE date_prod < 
'2016-05-07';
```

**Note:** If you want to insert a large amount of data, we recommend that you use an external table or execute a COPY statement to obtain better performance than is offered by an INSERT statement.

## Update data

You can execute the `UPDATE` statement to update one or more rows in a table. Syntax:

```
UPDATE [ONLY] table [[AS] alias]
   SET {column = {expression | DEFAULT} |
   (column [, ...]) = ({expression | DEFAULT} [, ...])} [, ...]
   [FROM fromlist]
   [WHERE condition | WHERE CURRENT OF cursor_name ]
```

For more information, visit [UPDATE](http://gpdb.docs.pivotal.io/6-14/ref_guide/sql_commands/UPDATE.html).

**Limits:**

-   The column that is defined as the distribution key cannot be updated.
-   The column that is defined as the partition key cannot be updated.
-   STABLE and VOLATILE functions are not allowed.
-   RETURNING clauses are not allowed.

**Example:**

Execute the following statement to change the rows in which the value in the price column falls within the range of 5 to 10:

```
UPDATE products SET price = 10 WHERE price = 5;
```

## Delete data

You can execute the `DELETE` statement to delete one or more rows from a table. Syntax:

```
DELETE FROM [ONLY] table [[AS] alias]
      [USING usinglist]
      [WHERE condition | WHERE CURRENT OF cursor_name ]
```

For more information, visit [DELETE](http://gpdb.docs.pivotal.io/6-14/ref_guide/sql_commands/DELETE.html).

**Limits:**

-   STABLE and VOLATILE functions are not allowed.
-   RETURNING clauses are not allowed.

**Examples:**

Execute the following statement to delete all rows in which the value in the price column is 10:

```
DELETE FROM products WHERE price = 10;
```

Execute the following statement to delete all rows from a table:

```
DELETE FROM products;
```

## Truncate data

You can execute the `TRUNCATE` statement to remove all rows from a table. Syntax:

```
TRUNCATE [TABLE] name [, ...] [CASCADE | RESTRICT]
```

For more information, visit [TRUNCATE](http://gpdb.docs.pivotal.io/6-14/ref_guide/sql_commands/TRUNCATE.html).

**Example:**

In the following example, all rows are removed from the mytable table by using a TRUNCATE statement. The TRUNCATE statement does not scan the mytable table. Therefore, the TRUNCATE statement does not process the child tables of the mytable table or the rewrite rules specified by ON DELETE clauses. Only rows in the mytable table are truncated.

```
TRUNCATE mytable;
```

