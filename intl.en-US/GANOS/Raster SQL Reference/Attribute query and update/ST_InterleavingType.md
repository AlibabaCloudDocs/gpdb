# ST\_InterleavingType

This function returns the interleaving type of a raster object. The interleaving type can be BSQ, BIL, or BIP.

## Syntax

```
text ST_InterleavingType(raster raster_obj);
```

## Parameters

|Parameter|Description|
|:--------|:----------|
|raster\_obj|The raster object.|

## Examples

```
select ST_InterleavingType(raster_obj) from raster_table;

__________________________________
BSQ
```

