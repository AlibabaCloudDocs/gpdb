# ST\_Envelope

This topic describes the ST\_Envelope function, which obtains the bounding box of a raster.

## Syntax

```
geometry ST_Envelope(raster source);
geometry ST_Envelope(raster source,
                     integer pyramid);
```

## Parameters

|Parameter|Description|
|:--------|:----------|
|source|The raster whose bounding box you want to obtain.|
|pyramid|The pyramid level at which the raster resides. Valid values start from 0. Default value: 0.|

## Description

This function obtains the bounding box of a raster.

![Bounding box](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/4480289951/p129348.png)

## Example

```
SELECT st_astext(st_envelope(rast_object))
FROM raster_table;

----------------------------------------------------
 POLYGON((-180 90,180 90,180 -90,-180 -90,-180 90))

--Specify the pyramid level at which the raster resides.                 
SELECT st_astext(st_envelope(rast_object,  1))
FROM raster_table;

----------------------------------------------------
 POLYGON((-180 90,180 90,180 -90,-180 -90,-180 90))  
```

