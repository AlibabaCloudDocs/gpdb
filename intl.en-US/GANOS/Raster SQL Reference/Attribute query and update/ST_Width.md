# ST\_Width

This topic describes the ST\_Width function, which obtains the width of a raster. For more information about how to obtain the width of a chunk, see the ST\_ChuckWidth function.

## Syntax

```
integer ST_Width(raster raster_obj);
integer ST_Width(raster raster_obj, integer pyramid);
```

## **Parameters**

|Parameter|Description|
|---------|-----------|
|raster\_obj|The raster whose width you want to obtain.|
|pyramid|The pyramid level at which the raster resides. Valid values start from 0.|

## Example

```
select ST_Width(raster_obj) from raster_table;
---------------------------------------------
10060
```

