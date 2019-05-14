# 如何使用 AnalyticDB for PostgreSQL 列存和压缩功能？ {#concept_dky_tlg_v2b .concept}

在 AnalyticDB for PostgreSQL 中创建表时，默认使用行式存储，并且不启用压缩。如需使用列存和压缩功能，您必须在建表时指定列存和压缩选项。例如，您可以在建表语句中加入以下子句，来启用列存和压缩功能。

```
with  (APPENDONLY=true, ORIENTATION=column, COMPRESSTYPE=zlib, COMPRESSLEVEL=5, BLOCKSIZE=1048576, OIDS=false)
```

一般情况下，推荐您使用列存和压缩功能，尤其是在包含较多复杂查询，或需要降低存储成本的场景。

**说明：** 目前，AnalyticDB for PostgreSQL 仅支持 zlib 和 RLE\_TYPE 压缩算法；如果指定了 quicklz 算法，会自动转换为 zlib。

