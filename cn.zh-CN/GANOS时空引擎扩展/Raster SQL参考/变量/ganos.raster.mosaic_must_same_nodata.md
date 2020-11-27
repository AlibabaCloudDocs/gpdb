# ganos.raster.mosaic\_must\_same\_nodata

指定镶嵌时数据源的NoData值是否必须相同。镶嵌时并不会对NoData值进行转换，如果该变量的取值为false时可能会导致镶嵌后的像素语义不一致。

## 类型

boolean

## 默认值

true

**说明：** 取值：true \| false。

## 示例

```
Set ganos.raster.mosaic_must_same_nodata = false;
```

