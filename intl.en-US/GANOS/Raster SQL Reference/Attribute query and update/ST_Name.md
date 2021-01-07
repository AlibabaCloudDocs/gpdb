# ST\_Name

This function returns the name of a raster object. If no name is specified, the function returns null.

## Syntax

```
text ST_Name(raster rast);
```

## Parameters

|Parameter|Description|
|---------|-----------|
|rast|The raster object.|

## Examples

```
select ST_Name(rast) from rat where id=1;

__________________________________
image1
```

