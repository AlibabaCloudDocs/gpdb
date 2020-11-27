# ST\_SetColorInterp

设置raster对象的指定波段的颜色解释类型。

## 语法

```
raster ST_SetColorInterp(raster rast, integer band_sn, ColorInterp interp)
```

## 参数

|参数名称|描述|
|----|--|
|rast|raster对象。|
|band\_sn|指定的波段序号，从0开始。|
|interp|interp枚举值。|

## 描述

interp枚举值及其解释：

|值|说明|
|--|--|
|Undefined|颜色解释类型未定义。|
|GrayIndex|关联灰度颜色表。|
|RGBIndex|关联RGB颜色表。|
|RGBAIndex|关联RGBA颜色表。|
|CMYKIndex|关联CMYK颜色表。|
|HSLIndex|关联HSL颜色表。|
|RedBand|红色波段。|
|GreenBand|绿色波段。|
|BlueBand|蓝色波段。|
|AlphaBand|透明波段。|
|HueBand|HLS的色调分量。|
|SaturationBand|HLS的饱和度分量。|
|LightnessBand|HLS的亮度分量。|
|CyanBand|CMYK的青色波段。|
|MegentaBand|CMYK的品红波段。|
|YellowBand|CMYK的黄色波段。|
|BlackBand|CMYK的黑色波段。|
|YBand|YCBCR的亮度分量。|
|CbBand|YCBCR的蓝色色度分量。|
|CrBand|YCBCR的红色色度分量。|

## 示例

```
update rast set rast=ST_SetColorInterp(rast,0, 'CI_Cyan');

__________________________________
(1 row)
```

