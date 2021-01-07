# ST\_Extent

This topic describes the ST\_Extent function, which obtains the coordinate range of a raster in the following format: `'((maxX,maxY),(minX,minY))'`.

## Syntax

```
BOX ST_Extent(raster raster_obj,CoorSpatialOption csOption = 'WorldFirst')
BOX ST_Extent(raster raster_obj, integer pyramid, CoorSpatialOption csOption = 'WorldFirst')
```

## Parameters

|Parameter|Description|
|:--------|:----------|
|raster\_obj|The raster whose coordinate range you want to obtain.|
|csOption|The type of coordinate to return.|
|pyramid|The pyramid level at which the raster resides. Valid values start from 0.|

## Description

This function obtains the coordinate range of a raster based on the value of the csOption parameter:

-   Raster: This function returns pixel coordinates.
-   World: This function returns world coordinates.
-   WorldFirst: This function preferentially returns world coordinates. If the raster is georeferenced, this function returns world coordinates. Otherwise, this function returns pixel coordinates.

## Example

```
select ST_Extent(raster_obj, 'Raster'::CoorSpatialOption) from raster_table;

__________________________________
((255, 255), (0, 0))
```

