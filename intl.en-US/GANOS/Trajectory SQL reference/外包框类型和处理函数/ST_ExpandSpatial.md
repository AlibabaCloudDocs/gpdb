# ST\_ExpandSpatial

This function is used to expand a bounding box to a specified spatial scope.

## Syntax

```
boxndf ST_ExpandSpatial(boxndf box, float8 len);
```

## Parameters

|Parameter|Description|
|---------|-----------|
|box|The bounding box that you want to expand.|
|len|The length to which you want to expand the bounding box.|

## Description

This function allows you to expand a bounding box to a specified spatial scope. If the bounding box has the x, y, and z dimensions, the lower bound is decreased by the value of the len parameter, and the upper bound is increased by the value of the len parameter.

## Example

```
Select ST_ExpandSpatial(ST_MakeBox2dt(0,0,'2000-01-02', 20,20, '2000-03-03'), 4);
                      st_expandspatial                       
-------------------------------------------------------------
 BOX2DT(-4 -4 2000-01-02 00:00:00,24 24 2000-03-03 00:00:00)
```

