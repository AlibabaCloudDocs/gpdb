# ST\_Clip

This function clips a raster object.

## Syntax

```
bytea ST_Clip(raster raster_obj,integer pyramidLevel, box extent, BoxType boxType);
bytea ST_Clip(raster raster_obj,integer pyramidLevel, box extent, BoxType boxType, integer destSrid);
record ST_Clip(raster raster_obj,
                     geometry geom,
                     integer pyramidLevel default 0,
                     cstring bands default '',
                     float8[] nodata default NULL,
                     cstring clipOption default '',
                     cstring storageOption default '',
                     out box outwindow,
                     out bytea rasterblob)
```

## Parameters

|Parameter|Description|
|:--------|:----------|
|raster\_obj|The raster object.|
|pyramidLevel|The pyramid level.|
|extent|The area to be clipped, in the format of `'((minX,minY),(maxX,maxY))'`.|
|boxType|The coordinate type of the area to be clipped. Valid values: -   Raster: pixel coordinates
-   World: world coordinates |
|destSrid|The spatial reference system identifier \(SRID\) of the output cell subset.|
|geometry|The geometry object used for clipping.|
|bands|The sequence numbers of bands to be clipped, in the format of `'0-2'` or `'1,2,3'`. The sequence number starts from 0. Default value: empty string \(`''`\). It indicates that all bands are to be clipped.|
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

The default clipping cache is 100 MB, which indicates that only 100 MB of data can be returned. To adjust the limit on the output size, you can use the ganos.raster.clip\_max\_buffer\_size parameter to set the size of the cache.

## Examples

```
DO $$
declare
    rast raster;
begin
    select raster_obj into rast from raster_table where id = 1;
    Select ST_Clip(rast, 0, '((128.980,30.0),(129.0,30.2))', 'World');
end;    
$$ LANGUAGE 'plpgsql';
```

