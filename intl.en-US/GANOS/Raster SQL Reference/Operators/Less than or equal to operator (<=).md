# Less than or equal to operator \(<=\)

This topic describes the less than or equal to operator \(<=\). This operator determines whether the universally unique identifier \(UUID\) of the first specified raster object is less than or equal to the UUID of the second.

## Syntax

```
bool  Operator <=(raster rast1, raster rast2);
```

## Parameters

|Parameter|Description|
|---------|-----------|
|rast1|The raster object 1.|
|rast2|The raster object 2.|

**Note:** This operator only compares the UUIDs of two specified raster objects. It does not compare their spatial extents or pixel types. This operator is used only for operations such as union and B-tree.

## Example

```
SELECT a.rast <= b.rast 
FROM tbl_a a, tbl_b b
WHERE a.id = b.id
```

