# Define storage models for tables

AnalyticDB for PostgreSQL supports two storage models for tables: row-oriented storage and column-oriented storage.

## Row-oriented table

By default, AnalyticDB for PostgreSQL uses the heap storage model in PostgreSQL to create row-oriented heap tables. Row-oriented tables are used for data that needs to be updated at a high frequency or written in real time by using the INSERT statement. A row-oriented table with a B-tree index provides high data retrieval performance when you query a small amount of data at a time.

**Example:**

The following statement creates a row-oriented heap table:

```
CREATE TABLE foo (a int, b text) DISTRIBUTED BY (a);
```

**Note:** When you use Data Transmission Service \(DTS\) to write data into your AnalyticDB for PostgreSQL instance, the destination tables must be row-oriented tables. DTS allows data synchronization in near real time. In addition to data inserted using the INSERT statement, DTS can synchronize data updated using SQL statements such as UPDATE and DELETE.

## Column-oriented table

Data within a column-oriented table is stored by column. When you access data, only relevant columns are read. Column-oriented tables are used in data warehousing scenarios such as data queries and aggregations of a small number of columns. In these scenarios, column-oriented tables provide efficient I/O. However, column-oriented tables are less efficient in scenarios that require frequent update operations or a large number of INSERT operations. We recommend that you use a batch loading method such as COPY to insert data into column-oriented tables. Column-oriented tables provide a data compression ratio three to five times higher than that provided by row-oriented tables.

**Example:**

Column-oriented tables must be append-optimized tables. This means that you must set the appendonly parameter to true for the column-oriented table you want to create.

```
CREATE TABLE bar (a int, b text) 
    WITH (appendonly=true, orientation=column)
    DISTRIBUTED BY (a);
```

## Data compression

Data compression is used for column-oriented tables or for append-optimized row-oriented tables whose appendonly parameter is set to true. There are two compression types:

-   Table-level compression.
-   Column-level compression. You can use a unique compression algorithm for each column.

AnalyticDB for PostgreSQL only supports the following compression algorithms:

-   AnalyticDB for PostgreSQL V4.3 supports zlib and RLE\_TYPE.
-   AnalyticDB for PostgreSQL V6.0 supports Zstandard \(zstd\), zlib, RLE\_TYPE, and lz4.

**Note:** If you specify the QuickLZ compression algorithm, zlib is used instead. RLE\_TYPE is only used for column-oriented tables.

**Examples:**

Create a column-oriented table that uses the zlib compression algorithm with a compression level of 5.

```
CREATE TABLE foo (a int, b text) 
   WITH (appendonly=true, orientation=column, compresstype=zlib, compresslevel=5);
```

Create a column-oriented table that uses the zstd compression algorithm with a compression level of 9.

```
CREATE TABLE foo (a int, b text) 
   WITH (appendonly=true, orientation=column, compresstype=zstd, compresslevel=9);
```

