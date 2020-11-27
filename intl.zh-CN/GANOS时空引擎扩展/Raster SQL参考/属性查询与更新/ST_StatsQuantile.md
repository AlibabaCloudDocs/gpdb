# ST\_StatsQuantile

计算raster对象的中位数。

## 语法

```
raster ST_StatsQuantile(raster raster_obj)
```

## 参数

|参数名称|描述|
|----|--|
|raster\_obj|raster对象。|

## 描述

按照波段计算中位数，计算结果会记录到raster对象的元数据中。

## 示例

```
DO $$
declare
    rast raster;
begin
    select raster_obj into rast from raster_table where id = 1;
    rast = ST_StatsQuantile(rast);
    update raster_table set raster_obj = rast where id = 1;
end;    
$$ LANGUAGE 'plpgsql';
```

