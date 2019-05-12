# 列存表排序键（SortKey）定义 {#concept_nrh_nd2_v2b .concept}

排序键（SortKey）是列存表的一种可选属性，可以将数据按照排序键顺序存储在磁盘文件中。使用排序键的优势主要有：

-   加速列存优化，收集的min、max元信息很少有重叠，过滤性好。
-   避免对含有order by和group by等需要排序的SQL数据再次排序，从磁盘中直接读取出来的数据就是满足条件的有序数据。

本文介绍了排序键在不同场景下的用法。

## 创建表时定义排序键 {#section_grg_np2_v2b .section}

使用命令`CREATE TABLE`定义一个新的表。语法如下:

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

**样例**

```
create table test(date text, time text, open float, high float, low float, volume int) with(APPENDONLY=true,ORIENTATION=column) sortkey (volume);
```

## 对表进行排序 {#section_urg_np2_v2b .section}

命令如下：

```
VACUUM SORT ONLY [tablename]
```

## 修改排序键 {#section_wrg_np2_v2b .section}

命令如下：

```
ALTER [[GLOBAL | LOCAL] {TEMPORARY | TEMP}] TABLE table_name SET SORTKEY (column, [ ... ] )
```

**说明：** 这个命令只会修改catalog，不会对数据立即排序，需要使用`VACUUM SORT ONLY`命令排序。

**样例**

```
alter table test set sortkey (high,low);
```

## 注意事项 {#section_zrg_np2_v2b .section}

一旦对表进行了更新（例如Insert、Update、Delete），表的数据将被视为未按排序键排序，查询中不会将直接从磁盘读出的数据视为有序数据。此时，需要重新执行`VACUUM SORT ONLY`命令重新整理表数据。

