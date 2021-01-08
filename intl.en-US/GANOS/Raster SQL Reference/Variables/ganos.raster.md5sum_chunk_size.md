# ganos.raster.md5sum\_chunk\_size

This variable specifies the size of data that can be cached for each read operation when the system calculates the MD5 hash string. Unit: MB.

## Type

integer

## Default value

10

**Note:** Valid values: 1 to 256.

## Examples

```
Set ganos.raster.md5sum_chunk_size = 20;
```

