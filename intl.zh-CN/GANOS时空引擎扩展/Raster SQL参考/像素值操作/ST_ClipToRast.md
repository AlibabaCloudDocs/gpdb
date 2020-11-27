# ST\_ClipToRast

用指定的Geometry对象去裁剪Raster对象，并将裁剪结果作为一个新的Raster对象返回。

## 语法

```
raster ST_ClipToRast(raster raster_obj,
                     geometry geom,
                     integer pyramidLevel default 0,
                     cstring bands default '',
                     float8[] nodata default NULL,
                     cstring clipOption default '',
                     cstring storageOption default '')
```

## 参数

|参数名称|描述|
|:---|:-|
|raster\_obj|需要裁剪的Raster对象。|
|pyramidLevel|金字塔层级。|
|geometry|用于裁剪的Geometry对象。|
|bands|需要裁剪的波段，用`'0-2'`或者`'1,2,3'`这种形式表示，以0开始。默认为`''`，表示裁剪所有的波段。|
|nodata|用float8\[\]表示的nodata数值。如果数值个数少于波段数量，则使用波段设置的nodata值填充。如果波段未设置nodata，则用0填充。|
|clipOption|JSON字符串表示的裁剪选项。|
|storageOption|JSON字符串表示的返回结果的存储选项。|

clipOption参数如下。

|参数名称|类型|默认值|描述|
|:---|--|---|:-|
|window\_clip|bool|false|是否使用Geometry的外包框进行裁剪。取值： -   true：使用Geometry的MBR裁剪。
-   false：使用Geometry对象裁剪。 |
|rast\_coord|bool|false|传入的Geometry是否使用的是象元坐标。如果是象元坐标，横坐标x表示象元的列号，纵坐标y表示象元的行号。|

storageOption参数如下。

|参数名称|类型|默认值|描述|
|:---|--|---|:-|
|chunking|boolean|和原始Raster一致|是否使用分块存储。|
|chunkdim|string|和原始Raster一致|分块的维度信息。在chunking=true时才有效。|
|chunktable|string|''|分块表名称。如果传入`''`值，则会产生一个随机表名临时块表用于存放数据。 该临时表只在当前会话中有效。如果需要保持一个可访问的裁剪对象，则需要指定块表名称。|
|compression|string|lz4|压缩算法类型。取值： -   none
-   jpeg
-   zlib
-   png
-   lzo
-   lz4 |
|quality|integer|75|压缩质量，只针对jpeg压缩算法。|
|interleaving|string|和原始Raster一致|交错方式。取值： -   bip：波段按像元交叉
-   bil：波段按行交叉
-   bsq：波段顺序 |
|endian|string|和原始Raster一致|字节序。取值： -   NDR：小字节序（Little endian）
-   XDR：大字节序（Big endian） |

## 描述

-   如果chunkTable传入为NULL或者`''`， 则会产生一个随机表名的临时块表用于存放数据，该临时表只在当前会话中有效。如果需要保持一个可访问的裁剪对象，则需要指定块表名称。
-   默认的裁剪缓存为100 MB，代表最多只能裁剪出100 MB大小的结果数据，如果需要调整返回结果大小，可使用参数`ganos.raster.clip_max_buffer_size`设置缓存的大小。

## 示例

```
DO $$
declare
    rast raster;
    new_rast raster;
begin
    -- 永久表
    CREATE TEMP TABLE rast_clip_result(id integer, rast raster);
    
    select raster_obj into rast from raster_table where id = 1;
    new_rast = ST_ClipToRast(rast, ST_geomfromtext('Polygon((0 0, 45 45, 90 45, 45 0, 0 0))', 4326), 0);
    Insert into rast_clip_result values(1, new_rast);
end;    
$$ LANGUAGE 'plpgsql';
```

