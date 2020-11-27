# ST\_DeletePyramid

删除影像金字塔。

## 语法

```
raster ST_deletePyramid(raster source);
```

## 参数

|参数名称|描述|
|----|--|
|source|需要删除金字塔的raster对象。|

## 描述

删除影像金字塔，重置影像元数据，删除金字塔块数据。

## 示例

```
DO $$
declare
    rast raster;
begin
    select raster_obj into rast from raster_table where id = 1;
    rast = ST_deletePyramid(rast);
    update raster_table set raster_obj = rast where id = 1;
end;    
$$ LANGUAGE 'plpgsql';
```

