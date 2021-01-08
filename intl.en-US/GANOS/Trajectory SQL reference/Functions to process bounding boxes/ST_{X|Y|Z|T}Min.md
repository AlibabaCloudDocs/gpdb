# ST\_\{X\|Y\|Z\|T\}Min

The function is used to query the minimum value of a bounding box in a specified dimension.

## Syntax

```
float8 ST_XMin(boxndf box);
float8 ST_YMin(boxndf box);
float8 ST_ZMin(boxndf box);
timestamp ST_TMin(boxndf box);
```

## Parameters

|Parameter|Description|
|---------|-----------|
|box|The bounding box whose minimum value in a specified dimension you want to query.|

## Description

This function allows you to query the minimum value of a bounding box in a specified dimension.

-   If the bounding box does not have the specified x, y, or z dimension, this function returns `NaN`.
-   If the bounding box does not have the specified t dimension, this function returns `-infinity`.

## Example

```
select ST_xmin(ST_MakeBox2dt(0,0,'2000-01-01'::timestamp, 20,20, '2020-01-01'::timestamp));
 st_xmin 
---------
       0

select ST_zmax(ST_MakeBox2dt(0,0,'2000-01-01'::timestamp, 20,20, '2020-01-01'::timestamp));
 st_zmin 
---------
     NaN

select ST_tmax(ST_MakeBox2d(0,0, 20,20));
  st_tmin  
-----------
 -infinity
```

