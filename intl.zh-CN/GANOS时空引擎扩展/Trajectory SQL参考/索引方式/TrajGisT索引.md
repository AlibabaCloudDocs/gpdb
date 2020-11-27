# TrajGisT索引

TrajGisT索引是GisT索引的扩展。

## 背景信息

在GisT基础上，TrajGisT提供额外两点优化：

-   TrajGisT对索引的开销估计进行了优化，当建立了多个索引时，TrajGisT可以比GisT更好地在不同索引之间进行选择。
-   TrajGist支持索引的向上兼容，即当索引所记录的信息不足以对查询进行精确判断时，仍然可以通过索引扫描得到一个粗粒度的结果。例如只建立了时间索引，但需要进行2维和时间的2DTIntersects操作时，可以用时间索引过滤掉和给定的时间段不相交的轨迹。

## 语法

```
CREATE INDEX [index_name] on table_name USING TRAJGIST(traj_col [operator_family]);
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
|trajgist\_ops\_z|对z轴范围建立索引，支持所有包含z轴的查询。|
|trajgist\_ops\_t|对t轴范围建立索引，支持所有包含t轴的查询。|
|trajgist\_ops\_2d|对x、y轴范围建立索引，支持所有包含x、y轴信息的查询。|
|trajgist\_ops\_2dt|对x、y、t轴范围建立索引，支持所有包含x、y、t轴的查询。|
|trajgist\_ops\_3d|对x、y、z轴范围建立索引，支持所有包含x、y、z轴的查询。|
|trajgist\_ops\_3dt|对x、y、z、t轴范围建立索引，支持以上全部查询。|

**说明：** TrajGist目前仅支持单列索引，不能与其它（非轨迹）列建立多列索引。

