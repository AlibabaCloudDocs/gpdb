# Migrate data to AnalyticDB for PostgreSQL by using the Copy command {#concept_wtt_rmr_52b .concept}

You can directly run the `\COPY` command to import local text file data to AnalyticDB for PostgreSQL. The premise is that the local text file must be formatted, such as using commas \(,\), colons \(:\) or special symbols as separators.

**Note:** 

-   Parallel writing of massive data is unavailable because the `\COPY` command performs serial data writing through the master node. If you want to parallelly write massive data, use the OSS-based data importing method instead.
-   The `\COPY` command is an action instruction of PostgreSQL. If you use the database instruction `COPY` rather than `\COPY`, note that in this case only `STDIN` is supported and `file` is not supported. That is because the “root user” does not have the super user permission to perform operations on the `file` format files.

Syntax of the `\COPY` command is as follows:

```
\COPY table [(column [, ...])] FROM {'file' | STDIN}
     [ [WITH] 
       [OIDS]
       [HEADER]
       [DELIMITER [ AS ] 'delimiter']
       [NULL [ AS ] 'null string']
       [ESCAPE [ AS ] 'escape' | 'OFF']
       [NEWLINE [ AS ] 'LF' | 'CR' | 'CRLF']
       [CSV [QUOTE [ AS ] 'quote'] 
            [FORCE NOT NULL column [, ...]]
       [FILL MISSING FIELDS]
       [[LOG ERRORS [INTO error_table] [KEEP] 
       SEGMENT REJECT LIMIT count [ROWS | PERCENT] ]
\COPY {table [(column [, ...])] | (query)} TO {'file' | STDOUT}
      [ [WITH] 
        [OIDS]
        [HEADER]
        [DELIMITER [ AS ] 'delimiter']
        [NULL [ AS ] 'null string']
        [ESCAPE [ AS ] 'escape' | 'OFF']
        [CSV [QUOTE [ AS ] 'quote'] 
             [FORCE QUOTE column [, ...]] ]
      [IGNORE EXTERNAL PARTITIONS ]
```

-   AnalyticDB for PostgreSQL also supports using JDBC that encapsulates the CopyIn method to run the COPY statements. For detailed method, see [Interface CopyIn](https://jdbc.postgresql.org/documentation/publicapi/org/postgresql/copy/CopyIn.html).
-   For the usage of COPY command, see [COPY](http://gpdb.docs.pivotal.io/4380/ref_guide/sql_commands/COPY.html).

