# ST\_IsGeoreferenced

This function specifies whether a raster object is geographically referenced. The return value is t or f in Boolean format.

## Syntax

```
boolean ST_IsGeoreferenced(raster raster_obj)
```

## Parameters

|Parameter|Description|
|:--------|:----------|
|raster\_obj|The raster object.|

## Description

In the returned result, t indicates true and f indicates false.

## Examples

```
select ST_IsGeoreferenced(raster_obj) from raster_table where id=1;

__________________________________
t
```

