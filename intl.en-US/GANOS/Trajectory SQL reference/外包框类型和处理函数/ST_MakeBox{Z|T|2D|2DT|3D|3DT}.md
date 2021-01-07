# ST\_MakeBox\{Z\|T\|2D\|2DT\|3D\|3DT\}

This function is used to build a bounding box based on specified parameter settings.

## Syntax

```
boxndf ST_MakeBoxZ(float8 zmin, float8 zmax);
boxndf ST_MakeBoxT(timestamp tmin, timestamp tmax);
boxndf ST_MakeBox2D(float8 xmin, float8 ymin, float8 xmax, float8 ymax);
boxndf ST_MakeBox2DT(float8 xmin, float8 ymin, timestamp tmin, float8 xmax,float8 yax, timestamp tmax);
boxndf ST_MakeBox3D(float8 xmin, float8 ymin, float8 zmin, float8 xmax, float8 ymax, float8 zmax);
boxndf ST_MakeBox3DT(float8 xmin, float8 ymin, float8 zmin, timestamp tmin, float8 xmax, float8 ymax, float8 zmax, timestamp tmax);
```

## Parameters

|Parameter|Description|
|---------|-----------|
|xmin|The lower bound on the x axis of the bounding box.|
|xmax|The upper bound on the x axis of the bounding box.|
|ymin|The lower bound on the y axis of the bounding box.|
|ymax|The upper bound on the y axis of the bounding box.|
|zmin|The lower bound on the z axis of the bounding box.|
|zmax|The upper bound on the z axis of the bounding box.|
|tmin|The lower bound on the t axis of the bounding box.|
|tmax|The upper bound on the t axis of the bounding box.|

## Description

This function allows you to build a bounding box based on the function name and the specified parameter settings.

Bounding boxes use the FLOAT data type. Therefore, this function may return a bounding box that is slightly larger than specified by the parameter settings. For example, the returned lower bound is slightly smaller than the actual lower bound, or the returned upper bound is slightly larger than the actual upper bound.

## Example

```
SELECT ST_MakeBox2d(0,0,3,3);
  st_makebox2d  
----------------
 BOX2D(0 0,3 3)
 
 SELECT ST_MakeBox3dt(0,0,3,'2000-01-01 00:00:03'::timestamp, 2,5,4,'2000-01-01 02:46:40'::timestamp);
                               st_makebox3dt                               
---------------------------------------------------------------------------
 BOX3DT(0 0 3 2000-01-01 00:00:02.999999,2 5 4 2000-01-01 02:46:40.000476)
(1 row)
```

