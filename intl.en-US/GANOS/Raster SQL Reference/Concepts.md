# Concepts

This topic introduces the basic concepts related to Raster SQL.

|Concept|Description|
|-------|-----------|
|raster object|The regular grid that represents a space. Each cell in a raster object is assigned an attribute value. A raster object can be a satellite image, a digital elevation model \(DEM\), or a picture.|
|cell or pixel|The cell in a raster object, which is also called a pixel. The data type of cells can vary with the raster object, such as byte, short, integer, or double.|
|band|The layer of the cell value matrix in a raster object. A raster object can have multiple bands.|
|chunk|The portion of a raster object. The size of a chunk can be customized, such as 256 × 256 × 3.|
|pyramid|The downsample version of a raster object. A pyramid can contain multiple downsample layers. Consecutive pyramid layers are downsampled at a scale of 2:1. Layer 0 stores the raw data.|
|pyramid level|The level in a pyramid.|
|mosaic|The operation to integrate multiple raster objects into an existing raster object.|
|interleaving|The arrangement of pixels in a raster object. The interleaving types include band sequential \(BSQ\), band interleaved by pixel \(BIP\), and band interleaved by line \(BIL\).|
|world space|The world coordinates \(geographic coordinates\) of a raster object.|
|raster space|The pixel coordinates of a raster object.|
|metadata|The metadata of a raster object, including the spatial range, projection type, and pixel type. The metadata of the remote sensing platform is excluded.|

