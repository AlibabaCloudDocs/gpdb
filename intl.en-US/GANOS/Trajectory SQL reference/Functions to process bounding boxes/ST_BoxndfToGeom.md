# ST\_BoxndfToGeom

This function is used to convert a bounding box into a geometry.

## Syntax

```
geometry ST_BoxNDFToGeom(boxndf box);
```

## Parameters

|Parameter|Description|
|---------|-----------|
|box|The bounding box that you want to convert into a geometry.|

## Description

This function allows you to convert a bounding box into a geometry.

-   If the bounding box has the z dimension, this function converts the bounding box into a three-dimensional geometry.
-   If the bounding box does not have the z dimension, this function converts the bounding box into a two-dimensional geometry.
-   If the bounding box does not have the x or y dimension, this function returns NULL.

## Example

```
select ST_AsText(ST_BoxndfToGeom(ST_MakeBox2dt(0,0,'2000-01-01'::timestamp, 20,20, '2020-01-01'::timestamp)));
             st_astext              
------------------------------------
 POLYGON((0 0,0 20,20 20,20 0,0 0))
```

