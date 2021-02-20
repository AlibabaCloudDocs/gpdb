# Migrate data from a Teradata database to an AnalyticDB for PostgreSQL instance

This topic describes how to migrate data from a Teradata database to an AnalyticDB for PostgreSQL instance.

## Migration requirements

AnalyticDB for PostgreSQL is compatible with Teradata syntax. To migrate data from a Teradata database to an AnalyticDB for PostgreSQL instance, you must convert DDL scripts on the basis of reusing original system architecture, Extract-Transform-Load \(ETL\) process, data structure, and management tools. You must also migrate historical data while ensuring data integrity and accuracy.

-   Perform complete migration for underlying data platform of the data warehouse.
-   Perform smooth migration for deployed applications on the data warehouse system.
-   Perform migration of which business users are not aware for business platforms and data to keep operations consistent across two systems.
-   Ensure good performance of the data warehouse after migration.
-   Set favorable migration duration and migration plans.
-   Fully reuse original system architecture, Extract-Transform-Load \(ETL\) process, data structure, and management tools.

1.  To migrate historical data, you must first export data in text files by using the specified delimiter and character encoding method. Then, store the exported files to local disks on ECS instances or OSS buckets. Local disks and OSS buckets must belong to the same network as the AnalyticDB for PostgreSQL instance. The preceding operations ensure that the AnalyticDB for PostgreSQL instance can read the files from the OSS external tables over the gpfdist protocol. Finally export DDL scripts from the Teradata database and batch modify the scripts by using the AnalyticDB for PostgreSQL syntax to make sure that all user tables can be created in the AnalyticDB for PostgreSQL instance.
2.  To migrate routine ETL processes, you must first convert ETL statements based on the Data Manipulation Language \(DML\) syntax of AnalyticDB for PostgreSQL. AnalyticDB for PostgreSQL provides the script-based tools that can automatically perform syntax mapping. Then, replace related functions based on the mapping between Teradata functions and AnalyticDB for PostgreSQL functions to transform the way to access databases by means of ETL operations. Reconfigure ETL operations and start routine ETL operations after successful migration of the historical data.
3.  The AnalyticDB for PostgreSQL instance supports JDBC and ODBC protocols. Business Intelligence \(BI\) frontend tools can access the data warehouse over JDBC and ODBC protocols. To migrate APIs, you only need to modify the IP address of the instance.
4.  To migrate management tools, you must deploy backup and restoration tools for the AnalyticDB for PostgreSQL instance to back up data and perform recovery drills on a regular basis.

The core data types of AnalyticDB for PostgreSQL and Teradata are mutually compatible with each other. Only some data types need to be modified. DDL statements for table creation are automatically converted in batches using the script-based tools in AnalyticDB for PostgreSQL. The following table lists data types in AnalyticDB for PostgreSQL and Teradata.

|Teradata|ADB PG|
|--------|------|
|CHAR|CHAR|
|VARCHAR|VARCHAR|
|LONG VARCHAR|VARCHAR\(64000\)|
|VARBYTE\(size\)|BYTEA|
|BYTEINT|BYTEA|
|SMALLINT|SMALLINT|
|INTEGER|INTEGER|
|DECIMAL\(size,dec\)|DECIMAL\(size,dec\)|
|NUMERIC\(precision,dec\)|NUMERIC\(precision,dec\)|
|FLOAT|FLOAT|
|REAL|REAL|
|DOUBLE PRECISION|DOUBLE PRECISION|
|DATE|DATE|
|TIME|TIME|
|TIMESTAMP|TIMESTAMP|

## Table creation statements

This section uses examples to describe the differences between AnalyticDB for PostgreSQL and Teradata.

To create a table in Teradata, execute the following statements:

```
CREATE MULTISET TABLE test_table,NO FALLBACK ,
     NO BEFORE JOURNAL,
     NO AFTER JOURNAL,
     CHECKSUM = DEFAULT,
     DEFAULT MERGEBLOCKRATIO
     (
      first_column DATE FORMAT 'YYYYMMDD' TITLE 'COLUMN 1' NOT NULL,
      second_column INTEGER TITLE 'COLUMN 2' NOT NULL ,
      third_column CHAR(6) CHARACTER SET LATIN CASESPECIFIC TITLE 'COLUMN 3' NOT NULL ,
      fourth_column CHAR(20) CHARACTER SET LATIN CASESPECIFIC TITLE 'COLUMN 4' NOT NULL,
      fifth_column CHAR(1) CHARACTER SET LATIN CASESPECIFIC TITLE 'COLUMN 5' NOT NULL,
      sixth_column CHAR(24) CHARACTER SET LATIN CASESPECIFIC TITLE 'COLUMN 6' NOT NULL,
      seventh_column VARCHAR(18) CHARACTER SET LATIN CASESPECIFIC TITLE 'COLUMN 7' NOT NULL,
      eighth_column DECIMAL(18,0) TITLE 'COLUMN 8' NOT NULL ,
      nineth_column DECIMAL(18,6) TITLE 'COLUMN 9' NOT NULL )
PRIMARY INDEX ( first_column ,fourth_column )
PARTITION BY RANGE_N(first_column  BETWEEN DATE '1999-01-01' AND DATE '2050-12-31' EACH INTERVAL '1' DAY );

CREATE INDEX test_index (first_column, fourth_column) ON test_table;
```

To create a table in AnalyticDB for PostgreSQL, execute the following statements:

```
CREATE TABLE test_table
     (
      first_column DATE NOT NULL,
      second_column INTEGER NOT NULL ,
      third_column CHAR(6) NOT NULL ,
      fourth_column CHAR(20) NOT NULL,
      fifth_column CHAR(1) NOT NULL,
      sixth_column CHAR(24) NOT NULL,
      seventh_column VARCHAR(18) NOT NULL,
      eighth_column DECIMAL(18,0) NOT NULL ,
      nineth_column DECIMAL(18,6) NOT NULL )
DISTRIBUTED BY ( first_column ,fourth_column )
PARTITION BY RANGE(first_column) 
(START (DATE '1999-01-01')  INCLUSIVE
END (DATE '2050-12-31')  INCLUSIVE
EVERY (INTERVAL '1 DAY' ) );

create index test_index on test_table(first_column, fourth_column);
```

Based on the preceding examples, the similarities and differences between table creation statements in AnalyticDB for PostgreSQL and Teradata are as follows:

-   Core data types are compatible with each other and require no modifications.
-   Both AnalyticDB for PostgreSQL and Teradata support distribution columns, but the syntax is different. The PRIMARY INDEX clause is used in Teradata, while DISTRIBUTED BY is used in AnalyticDB for PostgreSQL.
-   Both AnalyticDB for PostgreSQL and Teradata support the PARTITION BY clause. Such clauses in AnalyticDB for PostgreSQL and Teradata have the same semantics but different syntax.
-   Both AnalyticDB for PostgreSQL and Teradata allow you to create indexes on tables, but the syntax used is different.
-   AnalyticDB for PostgreSQL does not support the TITLE keyword, but allows you to add a comment to a specific column by executing the following statement: `COMMENT ON COLUMN table_name.column_name IS 'XXX';`.
-   AnalyticDB for PostgreSQL cannot declare the encoding type when you define the CHAR or VARCHAR data type. You can use the `SET client_encoding = latin1;` statement to declare the encoding type.

## Imported and exported data formats

Both AnalyticDB for PostgreSQL and Teradata allow you to import or export data in the TXT or CSV format. The difference lies in the separators used in data files.

-   Teradata uses two-character separators.
-   AnalyticDB for PostgreSQL uses single-character separators.

## SQL statements

AnalyticDB for PostgreSQL is compatible with the syntax of most SQL statements in Teradata. You only need to modify the following syntax:

-   **CAST**

    In Teradata:

    ```
    cast(XXX as int format '999999')
    cast(XXX as date format 'YYYYMMDD')
    ```

    In AnalyticDB for PostgreSQL:

    ```
    cast(XXX as int)
    cast(XXX as date)
    ```

    AnalyticDB for PostgreSQL does not declare a format in a CAST statement.

    -   For the `cast(XXX as int format '999999')` statement, you must write a function for replacement.
    -   You do not need to modify the `cast(XXX as date format 'YYYYMMDD')` statement because AnalyticDB for PostgreSQL supports the `'YYYY-MM-DD'` format for a date.
-   **QUALIFY**

    The QUALIFY keyword in Teradata is used to further filter the results of the sorting function based on user-specified conditions.

    Example:

    ```
    SELECT itemid, sumprice, RANK() OVER (ORDER BY sumprice DESC)
         FROM (SELECT a1.item_id, SUM(a1.sale)
               FROM sales AS a1 
               GROUP BY a1.itemID) AS t1 (itemid, sumprice) 
         QUALIFY RANK() OVER (ORDER BY sum_price DESC) <=100;
    ```

    AnalyticDB for PostgreSQL does not support the QUALIFY keyword. You must change an SQL statement with this keyword to a nested subquery.

    ```
    SELECT itemid, sumprice, rank from 
    (SELECT itemid, sumprice, RANK() OVER (ORDER BY sumprice DESC) as rank
         FROM (SELECT a1.item_id, SUM(a1.sale)
               FROM sales AS a1 
               GROUP BY a1.itemID) AS t1 (itemid,sumprice)
    )  AS a
    where rank <=100;
    ```

-   **MACRO**

    Teradata uses MACRO to execute a group of SQL statements. Example:

    ```
    CREATE MACRO Get_Emp_Salary(EmployeeNo INTEGER) AS ( 
       SELECT 
       EmployeeNo, 
       NetPay 
       FROM  
       Salary 
       WHERE EmployeeNo = :EmployeeNo; 
    );
    ```

    AnalyticDB for PostgreSQL does not support macro, but you can use the FUNCTION statement to implement the MACRO function of Teradata. Example:

    ```
    CREATE OR REPLACE FUNCTION Get_Emp_Salary(
            EmployeeNo INTEGER,
            OUT EmployeeNo INTEGER,
            OUT NetPay FLOAT
    ) returns setof record AS 
    $$
    
            SELECT EmployeeNo,NetPay 
            FROM Salary
            WHERE EmployeeNo = $1
    
    $$
     LANGUAGE SQL;
    ```


## Function mapping

Mapping between Teradata functions and AnalyticDB for PostgreSQL functions

|Teradata function|AnalyticDB for PostgreSQL function|Description|
|-----------------|----------------------------------|-----------|
|ZEROIFNULL|COALESCE|Handles the null values by converting them into zeroes for cumulative data.|
|NULLIFZERO|COALESCE|Replaces 0 values with NULL for cumulative data.|
|INDEX|POSITION|Returns the position \(integer number\) of a substring in a string.|
|ADD\_MONTHS|TO\_DATE|Adds or subtracts a specified number of months to or from the input date.|
|FORMAT|TO\_CHAR/TO\_DATE|Specifies the format of data.|
|CSUM|Subquery|Returns the cumulative sum of a value expression for each row in a partition.|
|MAVG|Subquery|Computes the moving average of a specified column based on a defined number of rows also known as the query width.|
|MSUM|Subquery|Computes the moving sum of a specified column based on a defined query width.|
|MDIFF|Subquery|Computes the moving difference of a specified column based on a defined query width.|
|QUALIFY|Subquery|Filters the results of the sorting function based on user-specified conditions.|
|CHAR/CHARACTERS|LENGTH|Specifies the number of characters.|

