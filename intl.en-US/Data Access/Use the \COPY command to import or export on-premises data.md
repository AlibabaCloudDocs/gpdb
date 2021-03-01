# Use the \\COPY command to import or export on-premises data

This topic describes how to run the `\COPY` command on the psql CLI client to import and export data between your computer and an AnalyticDB for PostgreSQL instance. Only .txt files can be imported from your computer to an AnalyticDB for PostgreSQL instance. In addition, they must be formatted. For example, they must use commas \(,\), semicolons \(;\), or other special characters as delimiters.

**Note:**

-   The `\COPY` command can only be executed by the coordinator node that is used to process serial data writes. It cannot be used to write a large volume of data in parallel. If you want to write a large volume of data in parallel, we recommend that you first import the data from your computer to Alibaba Cloud Object Storage Service \(OSS\) and then import the data from OSS to the AnalyticDB for PostgreSQL instance.

-   The `\COPY` command is executed on the psql CLI client. If you use the `COPY` statement but not the `\COPY` command, only standard input streams are supported and files are not supported because the root user does not have the superuser permissions to manage files.


The following example shows how to run the `\COPY` command in psql to import data from your computer to an AnalyticDB for PostgreSQL instance:

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
		
```

The following example shows how to run the `\COPY` command to export data from an AnalyticDB for PostgreSQL instance to your computer:

```





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

**Note:**

-   You can execute the COPY statement in AnalyticDB for PostgreSQL by using Java Database Connectivity \(JDBC\) that encapsulates the CopyIn method. For more information anout CopyIn, visit [Interface CopyIn](https://jdbc.postgresql.org/documentation/publicapi/org/postgresql/copy/CopyIn.html).

-   For more information about the COPY statement, visit [COPY](https://gpdb.docs.pivotal.io/6-1/ref_guide/sql_commands/COPY.html).


