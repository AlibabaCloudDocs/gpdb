# ST\_UpperLeftX

Queries the upper-left X coordinate of a raster object in the spatial reference system.

## Prerequisites

The raster object has a valid spatial reference system identifier \(SRID\).

## Syntax

```
float8 ST_UpperLeftX(raster raster_obj)
```

## Parameters

|Parameter|Description|
|---------|-----------|
|raster\_obj|The name of the raster object.|

## Examples

```
select st_upperleftx(rast), st_upperlefty(rast)
from raster_table;

 st_upperleftx | st_upperlefty 
---------------+---------------
        440720 |       3751320
```

