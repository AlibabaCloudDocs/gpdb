# ST\_StatsQuantile

This function calculates the quantiles of a raster object.

## Syntax

```
raster ST_StatsQuantile(raster raster_obj)
```

## Parameters

|Parameter|Description|
|---------|-----------|
|raster\_obj|The name of the raster object.|

## Description

This function calculates quantiles based on bands and records the quantiles in the metadata of the raster object.

## Examples

```
DO $$
declare
    rast raster;
begin
    select raster_obj into rast from raster_table where id = 1;
    rast = ST_StatsQuantile(rast);
    update raster_table set raster_obj = rast where id = 1;
end;    
$$ LANGUAGE 'plpgsql';
```

