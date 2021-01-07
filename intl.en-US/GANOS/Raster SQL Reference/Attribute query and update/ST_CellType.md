# ST\_CellType

This function returns the pixel type of a raster object. The pixel type can be 8BSI, 8BUI, 16BSI, 16BUI, 32BSI, 32BUI, 32BF, or 64BF.

## Syntax

```
text ST_CellType(raster raster_obj);
```

## Parameters

|Parameter|Description|
|:--------|:----------|
|raster\_obj|The raster object.|

## Examples

```
select st_celltype(raster_obj) from raster_table;

__________________________________
8BUI
```

