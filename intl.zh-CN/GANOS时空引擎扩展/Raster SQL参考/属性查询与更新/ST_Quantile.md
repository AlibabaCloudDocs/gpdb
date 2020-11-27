# ST\_Quantile

查询raster对象分位数的像素值。

## 前提条件

通过[ST\_StatsQuantile](/intl.zh-CN/GANOS时空引擎扩展/Raster SQL参考/属性查询与更新/ST_StatsQuantile.md)预先计算分位数。

## 语法

```
setof record ST_Quantile(raster raster_obj,
                   float8[] quantiles default NULL,
                   cstring bands default '',
                   boolean exclude_nodata_value default true, 
                   out integer band,
                   out float8 quantile,
                   out float8 value)
```

## 参数

|参数名称|描述|
|----|--|
|raster\_obj|raster对象。|
|quantiles|需要计算的分位数，取值为0.25、0.5和0.75中的一个或多个。|
|bands|需要计算的波段，格式为`'0-2'`或者`'1,2,3'`，从0开始。 默认为`''`，表示裁剪所有的波段。|
|exclude\_nodata\_value|是否需要计算nodata。|
|band|返回波段号。|
|quantile|返回分位数。|
|value|返回像素值。|

## 示例

```
-- 计算所有波段 0.25 分位数的像素值。
SELECT  (ST_Quantile(rast, ARRAY[0.25], '0-2', true)).* FROM rat_quantile WHERE id = 1;
 band | quantile | value 
------+----------+-------
    0 |     0.25 |    11
    1 |     0.25 |    10
    2 |     0.25 |    50
(3 rows)

-- 计算0波段所有分位数的像素值。
SELECT  (ST_Quantile(rast, NULL, '0', true)).* FROM rat_quantile WHERE id = 1;
 band | quantile | value 
------+----------+-------
    0 |     0.25 |    11
    0 |      0.5 |    11
    0 |     0.75 |    65
(3 rows)
```

