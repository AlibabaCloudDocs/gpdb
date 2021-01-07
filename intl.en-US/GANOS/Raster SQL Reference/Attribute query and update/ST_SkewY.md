# ST\_SkewY

Queries the skew of a raster object on the Y scale in the spatial reference system.

## Prerequisites

The raster object has a valid spatial reference system identifier \(SRID\).

## Syntax

```
float8 ST_SkewY(raster raster_obj)
```

## Parameters

|Parameter|Description|
|---------|-----------|
|raster\_obj|The name of the raster object.|

## Examples

```
select st_skewx(rast), st_skewy(rast)
from raster_table;

 st_skewx | st_skewy 
----------+-----------
      0.3 |      0.2
```

