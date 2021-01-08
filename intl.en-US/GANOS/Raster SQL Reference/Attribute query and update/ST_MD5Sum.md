# ST\_MD5Sum

This function queries the MD5 hash string of a raster object.

## Syntax

```
text ST_MD5Sum(raster raster_obj)
```

## Parameters

|Parameter|Description|
|---------|-----------|
|raster\_obj|The name of the raster object.|

## Description

The system searches for the metadata of the raster object to obtain the MD5 hash string. If the MD5 hash string cannot be found, the system returns a NULL value. Then, the system calculates the MD5 hash string based on the Alibaba Cloud Object Storage Service \(OSS\) path where the raster object is stored.

GUC ganos.raster.calculate\_md5: specifies whether to calculate the MD5 hash string and saves the string to the metadata when the raster object is stored. Default value: false. For more information, see [ganos.raster.calculate\_md5](/intl.en-US/GANOS/Raster SQL Reference/Variables/ganos.raster.calculate_md5.md).

GUC ganos.raster.md5sum\_chunk\_size: specifies the size of data that can be cached for each read operation when the system calculates the MD5 hash string. Unit: MB. Default value: 10. For more information, see [ganos.raster.md5sum\_chunk\_size](/intl.en-US/GANOS/Raster SQL Reference/Variables/ganos.raster.md5sum_chunk_size.md).

## Examples

```
SELECT ST_MD5SUM(rast)
FROM raster_table
WHERE id = 1;

              st_md5sum
-----------------------------------
 21f41fd983d3139c75b04bff2b7bf5c9
```

