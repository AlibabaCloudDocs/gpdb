# ganos.raster.mosaic\_must\_same\_nodata

This variable specifies whether the values of NoData in a data source must be the same during the mosaic operation. The values of NoData are not changed during the mosaic operation. If you set this parameter to false, the semantics of the pixels after the mosaic operation may be changed.

## Type

boolean

## Default value

true

**Note:** Valid values: true and false.

## Examples

```
Set ganos.raster.mosaic_must_same_nodata = false;
```

