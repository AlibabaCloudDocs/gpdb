# ST\_SetAKSecret

Sets the AccessKey secret that is used to access a raster object stored in Alibaba Cloud Object Storage Service \(OSS\).

## Prerequisites

The raster object is stored in OSS.

## Syntax

```
raster ST_SetAKSecret(raster raster_obj, text key, bool valid default true)
```

## Parameters

|Parameter|Description|
|---------|-----------|
|raster\_obj|The name of the raster object.|
|key|The AccessKey secret that is used to log on to OSS. If you set this parameter to NULL, the previous AccessKey secret is used.|
|valid|Specifies whether to verify the validity of the logon information.|

## Examples

```
UPDATE raster_table
SET rast = ST_SetAKSecret(rast, 'OSS_ACCESSKEY_SECRET');
```

