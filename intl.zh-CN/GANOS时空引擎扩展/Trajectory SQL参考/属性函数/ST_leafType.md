# ST\_leafType

获得轨迹的叶面类型。

## 语法

```
text ST_leafType(trajectory traj);
```

## 参数

|参数名称|描述|
|----|--|
|traj|需要获得类型的轨迹。|

目前只支持ST\_POINT类型。

## 示例

```
select st_leafType(traj) from traj where id = 3;
 st_leaftype
 ------------- 
STPOINT
(1 row)
```

