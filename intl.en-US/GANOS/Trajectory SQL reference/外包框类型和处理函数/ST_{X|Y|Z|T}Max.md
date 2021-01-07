# ST\_\{X\|Y\|Z\|T\}Max

This function is used to query the maximum value of a bounding box in a specified dimension.

## Syntax

```
float8 ST_XMax(boxndf box);
float8 ST_YMax(boxndf box);
float8 ST_ZMax(boxndf box);
timestamp ST_TMax(boxndf box);
```

## Parameters

|Parameter|Description|
|---------|-----------|
|box|The bounding box whose maximum value in a specified dimension you want to query.|

## Description

This function allows you to query the maximum value of a bounding box in a specified dimension.

-   If the bounding box does not have the specified x, y, or z dimension, this function returns `NaN`.
-   If the bounding box does not have the specified t dimension, this function returns `-infinity`.

## Example

```
select ST_tmax(ST_MakeBox2dt(0,0,'2000-01-01'::timestamp, 20,20, '2020-01-01'::timestamp));
       st_tmax       
---------------------
 2020-01-01 00:00:00

select ST_zmin(ST_MakeBox2dt(0,0,'2000-01-01'::timestamp, 20,20, '2020-01-01'::timestamp));
 st_zmin 
---------
     NaN

select ST_tmin(ST_MakeBox2d(0,0, 20,20));
  st_tmin  
-----------
 -infinity
```

