# ST\_leafCount

获得轨迹的叶子数量（轨迹点个数）。

## 语法

```
integer ST_leafCount(trajectory traj);
```

## 参数

|参数名称|描述|
|----|--|
|traj|需要获得类型的轨迹。|

## 示例

```
select st_leafCount(traj) from traj where id = 2;
 st_leafcount 
--------------           
16
```

