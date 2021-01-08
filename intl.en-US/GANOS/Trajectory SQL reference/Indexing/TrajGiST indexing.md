# TrajGiST indexing

This topic describes TrajGiST indexing, which is an extension to GiST indexing. You can create a TrajGiST index on a column that stores trajectory data.

## Background information

TrajGiST performs better than GiST in the following ways:

-   TrajGiST provides a better method that is used to estimate the overheads of indexes. If more than one index is created, TrajGiST can better select an index than GiST.
-   TrajGiST supports upward compatibility of indexes. If an accurate result cannot be obtained because the index does not provide sufficient information, you can still obtain a coarse-grained result by using an index-based scan. For example, you have created an index only on the t axis, but you want to call the ST\_\{2DT\}Intersects function. In this case, you can filter out the trajectories that do not fall within a specified time range based on the index on the t axis.

## Syntax

```
CREATE INDEX [index_name] on table_name USING TRAJGIST(traj_col [operator_family]);
```

-   index\_name: the name of the index. This parameter is optional.
-   table\_name: the name of the table to which the column belongs.
-   traj\_col: the name of the column.
-   operator\_family: the operator family that is used to create the index. Default value: trajgist\_ops\_3dt. This parameter is optional.

**Note:** The created index accelerates queries that are run by operators and the following functions: ST\_ndIntersect, ST\_ndDWithin, ST\_ndContains, and ST\_ndWithin.

## Supported operator families

The following table describes the operator families that are supported by GiST.

|Operator family|Description|
|---------------|-----------|
|trajgist\_ops\_z|Creates an index on the z axis. This type of index supports queries that cover only the z axis.|
|trajgist\_ops\_t|Creates an index on the t axis. This type of index supports queries that cover only the t axis.|
|trajgist\_ops\_2d|Creates an index on the x and y axes. This type of index supports queries that cover only the x and y axes.|
|trajgist\_ops\_2dt|Creates an index on the x, y, and t axes. This type of index supports queries that cover the x, y, and t axes.|
|trajgist\_ops\_3d|Creates an index on the x, y, and z axes. This type of index supports queries that cover the x, y, and z axes.|
|trajgist\_ops\_3dt|Creates an index on the x, y, z, and t axes. This type of index supports all of the queries that are supported by the preceding five operator families.|

**Note:** TrajGiST allows you to create an index on a single column. It does not allow you to create an index on multiple columns including the columns that store other types of data than trajectory data.

