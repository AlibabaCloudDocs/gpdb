# ST\_AddZ

根据栅格的波段值来对geometry的z值进行设置。

## 语法

```
geometry ST_AddZ(raster source,
                 geometry geom,
                 integer pyramid,
                 integer band);
```

## 参数

|参数名称|描述|
|:---|:-|
|source|需要计算的raster对象。|
|geom|需要查询的几何对象。|
|pyramid|栅格的金字塔层级值，从0开始，金字塔层级为N，金字塔层级值为0~N-1中的整数，默认值为0。|
|band|栅格的波段值，从0开始，波段数量为N时，波段值为0~N-1中的整数，默认值为0。|

## 描述

根据栅格的波段值设置geometry的z值。如果栅格设置了几何参考，几何对象按照地理坐标进行查询，否则按照像元坐标进行查询。

**说明：** 几何对象上的点必须完全在栅格对象内。

## 示例

```
DO $$
declare
    rast raster;
begin
    select raster_obj into rast from raster_table where id = 1;
    SELECT ST_AddZ(rast, ST_GeomFromText('POINT(120.5 30.6)', 4326), 0, 0);
end;    
$$ LANGUAGE 'plpgsql';
```

