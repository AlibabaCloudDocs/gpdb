# ST\_BoxndfToGeom

将外包框类型转化为Geometry类型。

## 语法

```
geometry ST_BoxNDFToGeom(boxndf box);
```

## 参数

|参数名称|描述|
|----|--|
|box|指定的外包框。|

## 描述

将外包框类型转化为Geometry类型。

-   如果外包框有z维度，将转化为三维Geometry类型。
-   如果外包框没有z维度，将转化为二维Geometry类型。
-   如果外包框不包含x、y维度，则返回NULL。

## 示例

```
select ST_AsText(ST_BoxndfToGeom(ST_MakeBox2dt(0,0,'2000-01-01'::timestamp, 20,20, '2020-01-01'::timestamp)));
             st_astext              
------------------------------------
 POLYGON((0 0,0 20,20 20,20 0,0 0))
```

