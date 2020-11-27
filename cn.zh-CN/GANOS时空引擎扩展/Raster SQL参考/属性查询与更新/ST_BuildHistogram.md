# ST\_BuildHistogram

计算一个raster对象的指定波段集的直方图信息。

## 语法

```
raster ST_BuildHistogram(raster raster_obj);
```

## 参数

|参数名称|描述|
|----|--|
|raster\_obj|Raster对象。|

## 示例

```
DO $$
declare
    rast raster;
begin
    select raster_obj into rast from raster_table where id = 1;
    rast = st_buildhistogram(rast);
    update raster_table set raster_obj = rast where id = 1;
end;    
$$ LANGUAGE 'plpgsql';
```

