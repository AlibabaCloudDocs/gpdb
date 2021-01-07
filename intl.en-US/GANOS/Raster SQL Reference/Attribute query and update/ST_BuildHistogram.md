# ST\_BuildHistogram

This function calculates the histogram of a band set of a raster object.

## Syntax

```
raster ST_BuildHistogram(raster raster_obj);
```

## Parameters

|Parameter|Description|
|---------|-----------|
|raster\_obj|The raster object.|

## Examples

```
DO $$
declare
    rast raster;
begin
    select raster_obj into rast from raster_table where id = 1;
    rast = st_buildhistogram(rast);
    update raster_table set raster_obj = rast where id = 1;
end;    
$$ LANGUAGE 'plpgsql';
```

