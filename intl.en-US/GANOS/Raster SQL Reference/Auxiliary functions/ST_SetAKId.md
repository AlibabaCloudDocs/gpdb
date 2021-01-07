# ST\_SetAKId

Sets the AccessKey ID that is used to access a raster object stored in Alibaba Cloud Object Storage Service \(OSS\).

## Prerequisites

The raster object is stored in OSS.

## Syntax

```
raster ST_SetAKId(raster raster_obj, text id, bool valid default true)
```

## Parameters

|Parameter|Description|
|---------|-----------|
|raster\_obj|The name of the raster object.|
|id|The AccessKey ID that is used to log on to OSS. If you set this parameter to NULL, the previous AccessKey ID is used.|
|valid|Specifies whether to verify the validity of the logon information.|

## Examples

```
UPDATE raster_table
SET rast = ST_SetAKId(rast, 'OSS_ACCESSKEY_ID');

SELECT ST_AKId(rast) from raster_table;
     st_akid      
------------------
 OSS_ACCESSKEY_ID
```

