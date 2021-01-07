# ST\_AKId

Queries the AccessKey ID that is used to access a raster object stored in Alibaba Cloud Object Storage Service \(OSS\). This function can be used with the function that is used to modify multiple AccessKey IDs at a time.

## Prerequisites

The raster object is stored in OSS.

## Syntax

```
text ST_AKId(raster raster_obj)
```

## Parameters

|Parameter|Description|
|---------|-----------|
|raster\_obj|The name of the raster object.|

## Examples

```
SELECT ST_AKId(rast) from raster_table;

     st_akid      
------------------
 OSS_ACCESSKEY_ID
```

