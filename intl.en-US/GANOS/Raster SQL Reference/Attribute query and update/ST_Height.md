# ST\_Height

This topic describes the ST\_Height function, which obtains the height of a raster.

## Syntax

```
integer ST_Height(raster raster_obj);
integer ST_Height(raster raster_obj, integer pyramid)
```

## Parameters

|Parameter|Description|
|:--------|:----------|
|raster\_obj|The raster whose height you want to obtain.|
|pyramid|The pyramid level at which the raster resides. Valid values start from 0.|

## Example

```
select ST_Height(raster_obj) from raster_table;
```

