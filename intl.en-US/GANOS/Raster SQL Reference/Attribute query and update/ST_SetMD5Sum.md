# ST\_SetMD5Sum

This function configures the MD5 hash string of a raster object.

## Syntax

```
text ST_SetMD5Sum(raster raster_obj, text md5sum)
```

## Parameters

|Parameter|Description|
|---------|-----------|
|raster\_obj|The name of the raster object.|
|md5sum|The MD5 hash string of the raster object.|

## Description

This function records the MD5 hash string in the metadata of the raster object. The MD5 hash string must be 32 characters in length and can contain only digits and lowercase letters.

GUC ganos.raster.calculate\_md5: specifies whether to calculate the MD5 hash string and saves the string to the metadata when the raster object is stored. For more information, see [ganos.raster.calculate\_md5](/intl.en-US/GANOS/Raster SQL Reference/Variables/ganos.raster.calculate_md5.md).

## Examples

```
SELECT ST_MD5SUM(ST_SetMD5Sum(rast,'21f41fd983d3139c75b04bff2b7bf5c9'))
FROM raster_table
WHERE id = 1;

              st_md5sum
-----------------------------------
 21f41fd983d3139c75b04bff2b7bf5c9
```

