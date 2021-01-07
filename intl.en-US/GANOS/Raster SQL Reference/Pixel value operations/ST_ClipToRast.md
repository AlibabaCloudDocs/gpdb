# ST\_ClipToRast

This function clips a raster object by using a specified geometry object and returns the clipped raster object.

## Syntax

```
raster ST_ClipToRast(raster raster_obj,
                     geometry geom,
                     integer pyramidLevel default 0,
                     cstring bands default '',
                     float8[] nodata default NULL,
                     cstring clipOption default '',
                     cstring storageOption default '')
```

## Parameters

|Parameter|Description|
|:--------|:----------|
|raster\_obj|The raster object.|
|pyramidLevel|The pyramid level.|
|geometry|The geometry object used for clipping.|
|bands|The sequence of bands to be clipped. Specify it in the format of '0-2' or '1,2,3'. The sequence number starts from 0. Default value: empty string \(''\). It indicates that all bands are to be clipped.|
|nodata|The array of NoData values in the format of float8\[\]. If the number of NoData values is fewer than the number of bands to be clipped, the predefined NoData value of a band is used to fill the area after the band is clipped. If a band has no predefined NoData value, the value 0 is used to fill the area after the band is clipped.|
|clipOption|The clipping options. The value is a JSON-formatted string.|
|storageOption|The storage options for the output. The value is a JSON-formatted string.|

The following table describes the clipOption parameters.

|Parameter|Type|Default value|Description|
|:--------|----|-------------|:----------|
|window\_clip|BOOLEAN|false|Specifies whether to use the bounding box of the geometry object to clip the raster object. Valid values: -   true: uses the minimum bounding rectangle \(MBR\) of the geometry object.
-   false: uses the geometry object. |
|rast\_coord|BOOLEAN|false|Specifies whether the geometry object to clip is in raster coordinates. If true, the x coordinate of the geometry represents the column number and the y coordinate represents the row number.|

The following table describes fields in the storageOption parameters.

|Parameter|Type|Default value|Description|
|:--------|----|-------------|:----------|
|chunking|BOOLEAN|Same as the original raster object|Specifies whether to store data by chunk.|
|chunkdim|STRING|Same as the original raster object|The dimension information of the chunk. This parameter takes effect only when the value of the chunking parameter is set to true.|
|chunktable|STRING|Empty string \(''\)|The name of the chunk table. By default, if you enter an empty string \(`''`\), a temporary chunk table with a random name is generated to store data. This temporary chunk table is only valid in the current session. To save the new raster object permanently, you must specify that you want to create a permanent chunk table in the chunktable field.|
|compression|STRING|lz4|The type of the compression algorithm. Valid values: -   none
-   jpeg
-   zlib
-   png
-   lzo
-   lz4 |
|quality|INTEGER|75|The compression quality. This parameter takes effect only when the value of the compression parameter is set to jpeg.|
|interleaving|STRING|Same as the original raster object|The interleaving type. Valid values: -   bip: band interleaved by pixel \(BIP\)
-   bil: band interleaved by line \(BIL\)
-   bsq: band sequential \(BSQ\) |
|endian|STRING|Same as the original raster object|The endian format. Valid values: -   NDR: little endian format
-   XDR: big endian format |

## Description

-   If no data is stored in the chunk table or the name of the chunk table is set to an empty string \(''\), a temporary chunk table with a random table name is generated to store data. This temporary table is only valid in the current session. To save the new raster object permanently, you must specify that you want to create a permanent chunk table in the chunktable field.
-   The default clipping cache is 100 MB, which indicates that only 100 MB of data can be returned. To adjust the limit on the output size, you can use the ganos.raster.clip\_max\_buffer\_size parameter to set the size of the cache.

## Examples

```
DO $$
declare
    rast raster;
    new_rast raster;
begin
    -- Create a permanent table.
    CREATE TEMP TABLE rast_clip_result(id integer, rast raster);
    
    select raster_obj into rast from raster_table where id = 1;
    new_rast = ST_ClipToRast(rast, ST_geomfromtext('Polygon((0 0, 45 45, 90 45, 45 0, 0 0))', 4326), 0);
    Insert into rast_clip_result values(1, new_rast);
end;    
$$ LANGUAGE 'plpgsql';
```

