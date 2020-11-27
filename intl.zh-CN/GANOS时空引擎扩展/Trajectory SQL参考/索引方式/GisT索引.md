# GisT索引

对轨迹数据列创建GisT索引。

## 语法

```
CREATE INDEX [index_name] on table_name USING GIST(traj_col [operator_family]);
```

-   index\_name：索引名，可以省略。
-   table\_name：表名。
-   traj\_col：轨迹列名。
-   operator\_family：指定索引所使用的算子族，可以省略，默认值为trajgist\_ops\_3dt。

**说明：** 建立索引后，可以加速各类算子以及ST\_ndIntersect、ST\_ndDWithin、ST\_ndContains、ST\_ndWithin函数的查询。

## 支持算子族

当前支持6个索引的算子族，说明如下。

|算子族名称|描述|
|-----|--|
|trajgist\_ops\_z|对z轴范围建立索引，支持仅包含z轴的查询。|
|trajgist\_ops\_t|对t轴范围建立索引，支持仅包含t轴的查询。|
|trajgist\_ops\_2d|对x、y轴范围建立索引，支持仅包含x、y轴信息的查询。|
|trajgist\_ops\_2dt|对x、y、t轴范围建立索引，支持二维、时间，以及混合查询。|
|trajgist\_ops\_3d|对x、y、z轴范围建立索引，支持二维、三维、z轴查询。|
|trajgist\_ops\_3dt|对x、y、z、t轴范围建立索引，支持以上全部查询。|

**说明：** 对于同一个查询，一般来说索引所包含的维度越少，最终的查询效率越高。因此当频繁使用某类查询时，建议在默认的trajgist\_ops\_3dt之外建立对应维度的查询。

## 示例

```
--建立时间维索引。
CREATE INDEX on table_name USING GIST(traj_col trajgist_ops_t);

--建立二维索引。
CREATE INDEX on table_name USING GIST(traj_col trajgist_ops_2d);

--建立二维时空复合索引。
CREATE INDEX on table_name USING GIST(traj_col trajgist_ops_2dt);
```

