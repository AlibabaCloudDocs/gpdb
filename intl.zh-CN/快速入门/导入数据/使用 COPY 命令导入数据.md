# 使用 COPY 命令导入数据 {#concept_wtt_rmr_52b .concept}

用户可以直接使用`\COPY`命令，将本地的文本文件数据导入云数据库 HybridDB for PostgreSQL。但要求用户本地的文本文件是格式化的，如通过逗号、分号或特有符号作为分割符号的文件。

**说明：** 

-   由于`\COPY`命令需要通过 Master 节点进行串行数据写入处理，因此无法实现并行写入大批量数据。如果要进行大量数据的并行写入，请使用基于 OSS 的数据导入方式。

-   `\COPY`命令是 psql 的操作指令，如果您使用的不是`\COPY`，而是数据库指令`COPY`，则需要注意只支持 STDIN，不支持 file，因为“根用户”并没有 superuser 权限，不可以进行 file 文件操作。


`\COPY`操作命令参考如下：

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

**说明：** 

-   云数据库 HybridDB for PostgreSQL 还支持用户使用 JDBC 执行 COPY 语句，JDBC 中封装了 CopyIn 方法，详细用法请参见文档[Interface CopyIn](https://jdbc.postgresql.org/documentation/publicapi/org/postgresql/copy/CopyIn.html)。

-   COPY 命令使用方法请参见文档[COPY](http://gpdb.docs.pivotal.io/4380/ref_guide/sql_commands/COPY.html)。


