# ST\_\{X\|Y\|Z\|T\}Max

获取BoxNDF类型中指定维度的最大值。

## 语法

```
float8 ST_XMax(boxndf box);
float8 ST_YMax(boxndf box);
float8 ST_ZMax(boxndf box);
timestamp ST_TMax(boxndf box);
```

## 参数

|参数名称|描述|
|----|--|
|box|指定的外包框。|

## 描述

获取外包框类型中指定维度的最大值。如果外包框不包含指定维度时：

-   不包含x维度、y维度和z维度：返回`NaN`。
-   不包含t维度：返回`-infinity`。

## 示例

```
select ST_tmax(ST_MakeBox2dt(0,0,'2000-01-01'::timestamp, 20,20, '2020-01-01'::timestamp));
       st_tmax       
---------------------
 2020-01-01 00:00:00

select ST_zmin(ST_MakeBox2dt(0,0,'2000-01-01'::timestamp, 20,20, '2020-01-01'::timestamp));
 st_zmin 
---------
     NaN

select ST_tmin(ST_MakeBox2d(0,0, 20,20));
  st_tmin  
-----------
 -infinity
```

