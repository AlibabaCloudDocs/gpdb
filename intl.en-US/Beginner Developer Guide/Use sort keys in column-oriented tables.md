# Use sort keys in column-oriented tables

**Note:**

-   Only AnalyticDB for PostgreSQL V4.3 supports sort keys.
-   If you use AnalyticDB for PostgreSQL V6.0 or other versions, ignore the sort key feature.

A sort key is an optional attribute of a column-oriented table. Values in the sort key columns are stored on a disk in sorted order. Sort keys provide the following benefits:

-   Speed up and optimize column-oriented tables. The min and max meta information that the system collects seldom overlaps with each other, which offers good filterability.
-   Eliminate the need to perform ORDER BY and GROUP BY operations. The data directly read from the disk is ordered as required by the sorting conditions.

This topic describes how to use sort keys in different scenarios.

## Define columns as sort keys when you create a table

You can use the `CREATE TABLE` statement to create a table. The following statement shows the syntax of CREATE TABLE:

```
CREATE [[GLOBAL | LOCAL] {TEMPORARY | TEMP}] TABLE table_name (
[ { column_name data_type [ DEFAULT default_expr ]     [column_constraint [ ... ]
[ ENCODING ( storage_directive [,...] ) ]
]
   | table_constraint
   | LIKE other_table [{INCLUDING | EXCLUDING}
                      {DEFAULTS | CONSTRAINTS}] ...}
   [, ... ] ]
   [column_reference_storage_directive [, ] ]
   )
   [ INHERITS ( parent_table [, ... ] ) ]
   [ WITH ( storage_parameter=value [, ... ] )
   [ ON COMMIT {PRESERVE ROWS | DELETE ROWS | DROP} ]
   [ TABLESPACE tablespace ]
   [ DISTRIBUTED BY (column, [ ... ] ) | DISTRIBUTED RANDOMLY ]
   [ SORTKEY (column, [ ... ] )]
   [ PARTITION BY partition_type (column)
       [ SUBPARTITION BY partition_type (column) ]
          [ SUBPARTITION TEMPLATE ( template_spec ) ]
       [...]
    ( partition_spec )
        | [ SUBPARTITION BY partition_type (column) ]
          [...]
    ( partition_spec
      [ ( subpartition_spec
           [(...)]
         ) ]
    )
```

**Example**

```
create table test(date text, time text, open float, high float, low float, volume int) with(APPENDONLY=true,ORIENTATION=column) sortkey (volume);
```

## Sort a table

Use the following statement:

```
VACUUM SORT ONLY [tablename]
```

## Modify sort keys

Use the following statement:

```
ALTER [[GLOBAL | LOCAL] {TEMPORARY | TEMP}] TABLE table_name SET SORTKEY (column, [ ... ] )
```

**Note:** The preceding statement only modifies the catalog and does not sort data. You must execute the `VACUUM SORT ONLY` statement to sort data.

**Example**

```
alter table test set sortkey (high,low);
```

## Notes

After you perform operations such as INSERT, UPDATE, and DELETE on a table, data in the table is viewed as not sorted by the sort key. The data read from the disk is not viewed as sorted data in queries. You must execute the `VACUUM SORT ONLY` statement to rearrange the data in the table.

