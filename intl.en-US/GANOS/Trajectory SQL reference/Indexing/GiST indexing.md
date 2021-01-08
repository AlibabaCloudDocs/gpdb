# GiST indexing

This topic describes GiST indexing. You can create a GiST index on the column that stores trajectory data.

## Syntax

```
CREATE INDEX [index_name] on table_name USING GIST(traj_col [operator_family]);
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
|trajgist\_ops\_2dt|Creates an index on the x, y, and t axes. This type of index supports queries that cover only the x and y axes, queries that cover only the t axis, and queries that cover the x, y, and t axes.|
|trajgist\_ops\_3d|Creates an index on the x, y, and z axes. This type of index supports queries that cover only the x and y axes, queries that cover only the z axis, and queries that cover the x, y, and z axes.|
|trajgist\_ops\_3dt|Creates an index on the x, y, z, and t axes. This type of index supports all of the queries that are supported by the preceding five operator families.|

**Note:** In most cases, a smaller number of dimensions in the created index indicates a higher speed per query. If you frequently run a specific type of query, we recommend that you specify another operator family that meets your dimension requirements in addition to the default trajgist\_ops\_3dt operator family.

## Example

```
-- Create an index on the t axis.
CREATE INDEX on table_name USING GIST(traj_col trajgist_ops_t);

-- Create an index on the x and y axes.
CREATE INDEX on table_name USING GIST(traj_col trajgist_ops_2d);

-- Create an index on the x, y, and t axes.
CREATE INDEX on table_name USING GIST(traj_col trajgist_ops_2dt);
```

