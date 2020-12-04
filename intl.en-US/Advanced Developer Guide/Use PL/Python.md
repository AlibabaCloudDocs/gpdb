# Use PL/Python

AnalyticDB for PostgreSQL allows you to create user-defined functions \(UDFs\) in the PL/Python procedural language.

## Limits

-   AnalyticDB for PostgreSQL does not support trigger functions in PL/Python.
-   You cannot use cursors to update data. For example, you cannot use `UPDATE...WHERE CURRENT OF` or `DELETE...WHERE CURRENT OF`.
-   AnalyticDB for PostgreSQL supports Python 2 only.

## Create or delete a PL/Python plug-in

To create or delete a PL/Python plug-in, execute one of the following statements on an AnalyticDB for PostgreSQL instance. You do not have to execute the same statement twice on each database. PL/Python is considered an untrusted language. In this case, you must execute the statements by using a privileged account. In the following statements, the privileged account is admin, and the database on which you want to execute statements is testdb.

Create a PL/Python plug-in

```
$ psql -U admin -d testdb -c 'CREATE EXTENSION plpythonu;'
```

Delete a PL/Python plug-in

```
$ psql -U admin -d testdb -c 'DROP EXTENSION plpythonu;'
```

## Use PL/Python to create functions

**Note:** For security considerations, you do not have the permissions to create functions in PL/Python. To create such a function, [submit a ticket](https://workorder.console.aliyun.com/console.htm#/ticket/add?productCode=gpdb). After your request is accepted, technical engineers help you create the function.

Create a function in PL/Python

```
CREATE FUNCTION return_int_array()
  RETURNS int[]
AS $$
  return [1, 2, 3, 4]
$$ LANGUAGE plpythonu;
```

Call the function

```
SELECT return_int_array();
return_int_array
---------------------
{1,11,21,31}
(1 row) 
```

## References

For more information about how to use Python, visit the [Python official website](https://www.python.org/).

For more information about how to use PL/Python, see [PostgreSQL documentation](https://www.postgresql.org/docs/9.4/plpython.html).

