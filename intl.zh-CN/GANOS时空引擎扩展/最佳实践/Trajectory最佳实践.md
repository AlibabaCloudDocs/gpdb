# Trajectory最佳实践

## 使用合适的时空索引

合适的索引能加快查询速度，需要根据应用场景的要求来构建合适的索引。可针对轨迹数据类型构建以下索引：

-   空间索引：只针对轨迹的空间范围建立索引，适合只查询轨迹空间范围情况。
-   时间索引：只针对轨迹的时间范围建立索引，适合只查询轨迹时间范围情况。
-   时空复合索引：建立时空联合索引，适合时间和空间同时过滤查询。

```
--创建基于函数的空间索引，加速空间过滤
create index tr_spatial_geometry_index on trajtab using gist (st_trajectoryspatial(traj));


--创建基于函数的时间段索引，加速时间过滤
create index tr_timespan_time_index on trajtab using gist (st_timespan(traj));

--创建基于函数的轨迹起止时间
create index tr_starttime_index on trajtab using btree (st_starttime(traj));
create index tr_endtime_index on trajtab using btree (st_endtime(traj));


--首先创建btree_gist扩展
create extension btree_gist;

--建立btree_gist 起始时间、终止时间、空间复合索引
create index tr_traj_test_stm_etm_sp_index on traj_test using gist (st_starttime(traj),st_endtime(traj),st_trajectoryspatial(traj));
```

## 采用合理的分区表

随着使用时间的增加，数据库中的轨迹数据量也不断增加，导致数据库索引变大，查询变慢。您可考虑采用分区表的模式降低单表数据量。

使用分区表请参见PostgreSQL文档中[分区表](https://www.postgresql.org/docs/current/ddl-partitioning.html)相关章节。

## 减少使用字符串类型属性

轨迹属性中如有大量的字符串属性，会导致存储空间浪费和性能下降。

-   如果字符串为固定内容，可以转换为整型进行枚举，建议在程序中进行转换；
-   如果无法避免使用字符串类型属性，可指定字符串类型默认长度，避免空间浪费。

    设置字符串类型默认长度方法如下：

    ```
    -- 设置字符串类型默认长度为32
    Set ganos.trajectory.attr_string_length = 32；
    ```


## 采用批量轨迹生成模式

使用批量轨迹点生成轨迹模式，避免采取单个轨迹点追加模式。

## 采用优化压缩模式

lz4 压缩算法是一种优秀的压缩算法，具有更高的压缩率和更快的执行速度。如果要启用lz4压缩算法，设置方法如下：

```
-- 设置使用lz4 压缩
Set toast_compression_use_lz4 = true; 

-- 使用pg默认压缩算法
Set toast_compression_use_lz4 = false;
```

如果要对整个数据默认采用lz4压缩算法，设置方法如下：

```
-- 数据库使用lz4压缩
Alter database dbname Set toast_compression_use_lz4 = true;

-- 数据库使用默认压缩
Alter database dbname Set toast_compression_use_lz4 = false;
```

