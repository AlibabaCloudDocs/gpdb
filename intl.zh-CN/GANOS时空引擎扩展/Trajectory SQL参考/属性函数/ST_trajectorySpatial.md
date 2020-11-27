# ST\_trajectorySpatial

获得轨迹的几何对象。

## 语法

```
geometry ST_trajectorySpatial(trajectory traj);
geometry ST_trajSpatial(trajectory traj);
```

## 参数

|参数名称|描述|
|----|--|
|traj|需要获得几何对象的轨迹。|

## 示例

```
Select ST_AsText(ST_trajectorySpatial(traj)) FROM traj_table;
            st_astext
----------------------------------
 LINESTRING(114 35,115 36,116 37)
```

