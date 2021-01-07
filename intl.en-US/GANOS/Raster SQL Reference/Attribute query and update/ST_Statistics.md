# ST\_Statistics

This function returns the statistics about a band of a raster object in JSON format. If the band does not have statistics, the function returns null.

## Syntax

```
text ST_Statistics(raster raster_obj, integer band);
```

## Parameters

|Parameter|Description|
|---------|-----------|
|raster\_obj|The raster object.|
|band|The band sequence number, starting from 0.|

## Examples

```
select ST_Statistics(raster_obj, 0) from raster_table where id=1;

__________________________________
'{	"min": 0.00, "max": 255.00, "mean": 125.00, "std": 23.123, "approx": false}' 
```

