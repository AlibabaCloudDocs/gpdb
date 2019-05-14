# How to enable column-store and compression in AnalyticDB for PostgreSQL? {#concept_dky_tlg_v2b .concept}

Row-store with no compression is used by default when you create a table in AnalyticDB for PostgreSQL. To enable column-store and compression, you must specify the column-store and compression options when creating the table. For example, you can add the following statement to the tabulation statements to enable the two features.

```
with (APPENDONLY=true, ORIENTATION=column, COMPRESSTYPE=zlib, COMPRESSLEVEL=5, BLOCKSIZE=1048576, OIDS=false)
```

In general, we recommend that you enable column-store and compression, especially when you have a lot of complicated queries to the table, or when you want to reduce the storage cost.

**Note:** 

AnalyticDB for PostgreSQL currently only supports the zlib and RLE\_TYPE compression algorithms. If you specify to use quicklz, AnalyticDB for PostgreSQL automatically converts the algorithm to zlib.

