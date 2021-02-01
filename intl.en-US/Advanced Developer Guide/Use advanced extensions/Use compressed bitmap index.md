# Use compressed bitmap index

This topic describes how to use Roaring bitmaps in AnalyticDB for PostgreSQL. A bitmap is a common data structure that consists of values 0 and 1. A separate bitmap is created to house all possible values for each column. Each bit indicates whether a data row has a value in that column. A bitmap helps you check whether a value exists. It also enables you to expedite bitmap-related computing by using bitwise operations such as AND, OR, and ANDNOT. In the big data discipline, bitmaps are suitable for data queries and correlated computing workloads such as removing duplicate data, filtering data based on tags, and generating time series.

A conventional bitmap occupies a large amount of memory resources. Therefore, we recommend that you use compressed bitmaps. Roaring bitmaps are efficient compressed bitmaps that are used in almost all popular programming languages on various big data platforms.

## Overview

In a Roaring bitmap, a 32-bit integer within the range of \[0, n\] is divided into two parts: the most significant 16 bits comprise a 2^16 chunk and the least significant 16 bits are stored in a container. The containers of a Roaring bitmap are stored in a dynamic array as the primary index. AnalyticDB for PostgreSQL supports two types of containers: array containers and bitmap containers. An array container is used to store sparse data while a bitmap container is used to store dense data. An array container can store up to 4,096 integers. A bitmap container can store more than 4,096 integers.

Based on the preceding storage structure, AnalyticDB for PostgreSQL can rapidly retrieve a specific value from a Roaring bitmap. Roaring bitmaps provide algorithms suitable for bitwise operations such as AND, OR, and XOR between the two types of containers. This enables Roaring bitmaps to deliver excellent storage and computing performance.

For more information, visit [Roaring Bitmaps](http://roaringbitmap.org/).

## Procedure

Create the RoaringBitmap extension.

```
CREATE EXTENSION roaringbitmap;
```

Create a table with the roaringbitmap data type.

```
CREATE TABLE t1 (id integer, bitmap roaringbitmap);
```

Call the rb\_build function to insert data of the roaringbitmap type.

```
-- Set the bit value of a data record in the array to 1.
INSERT INTO t1 SELECT 1,RB_BUILD(ARRAY[1,2,3,4,5,6,7,8,9,200]);
-- Set the bit values of multiple elements to 1 and aggregate the bit values into a Roaring bitmap.  
INSERT INTO t1 SELECT 2,RB_BUILD_AGG(e) FROM GENERATE_SERIES(1,100) e;
```

Perform bitwise operations such as OR, AND, XOR, and ANDNOT.

```
SELECT RB_OR(a.bitmap,b.bitmap) FROM (SELECT bitmap FROM t1 WHERE id = 1) AS a,(SELECT bitmap FROM t1 WHERE id = 2) AS b;
```

Perform bitwise operations such as OR, AND, XOR, and BUILD to aggregate data and generate a new Roaring bitmap.

```
SELECT RB_OR_AGG(bitmap) FROM t1;
SELECT RB_AND_AGG(bitmap) FROM t1;
SELECT RB_XOR_AGG(bitmap) FROM t1;
SELECT RB_BUILD_AGG(e) FROM GENERATE_SERIES(1,100) e;
```

Calculate the cardinality of the Roaring bitmap. The cardinality indicates how many bit values are 1 in the Roaring bitmap.

```
SELECT RB_CARDINALITY(bitmap) FROM t1;
```

Obtain the subscripts of the bits whose values are 1.

```
SELECT RB_ITERATE(bitmap) FROM t1 WHERE id = 1;
```

## Bitmap calculation functions

|Function name|Input data|Message|Description|Examples|
|-------------|----------|-------|-----------|--------|
|rb\_build|integer\[\]|roaringbitmap|Creates a Roaring bitmap from an integer array.|```
rb_build('{1,2,3,4,5}')
``` |
|rb\_and|roaringbitmap,roaringbitmap|roaringbitmap|Performs an AND operation.|```
rb_and(rb_build('{1,2,3}'),rb_build('{3,4,5}'))
``` |
|rb\_or|roaringbitmap,roaringbitmap|roaringbitmap|Performs an OR operation.|```
rb_or(rb_build('{1,2,3}'),rb_build('{3,4,5}'))
``` |
|rb\_xor|roaringbitmap,roaringbitmap|roaringbitmap|Performs an XOR operation.|```
rb_xor(rb_build('{1,2,3}'),rb_build('{3,4,5}'))
``` |
|rb\_andnot|roaringbitmap,roaringbitmap|roaringbitmap|Performs an ANDNOT operation.|```
rb_andnot(rb_build('{1,2,3}'),rb_build('{3,4,5}'))
``` |
|rb\_cardinality|roaringbitmap|integer|Calculates the cardinality of a Roaring bitmap.|```
rb_cardinality(rb_build('{1,2,3,4,5}'))
``` |
|rb\_and\_cardinality|roaringbitmap,roaringbitmap|Integer|Calculates the cardinality from an AND operation on two Roaring bitmaps.|```
rb_and_cardinality(rb_build('{1,2,3}'),rb_build('{3,4,5}'))
``` |
|rb\_or\_cardinality|roaringbitmap,roaringbitmap|Integer|Calculates the cardinality from an OR operation on two Roaring bitmaps.|```
rb_or_cardinality(rb_build('{1,2,3}'),rb_build('{3,4,5}'))
``` |
|rb\_xor\_cardinality|roaringbitmap,roaringbitmap|Integer|Calculates the cardinality from an XOR operation on two Roaring bitmaps.|```
rb_xor_cardinality(rb_build('{1,2,3}'),rb_build('{3,4,5}'))
``` |
|rb\_andnot\_cardinality|roaringbitmap,roaringbitmap|Integer|Calculates the cardinality from an ANDNOT operation on two Roaring bitmaps.|```
rb_andnot_cardinality(rb_build('{1,2,3}'),rb_build('{3,4,5}'))
``` |
|rb\_is\_empty|roaringbitmap|boolean|Checks whether a Roaring bitmap is empty.|```
rb_is_empty(rb_build('{1,2,3,4,5}'))
``` |
|rb\_equals|roaringbitmap,roaringbitmap|boolean|Checks whether two Roaring bitmaps are the same.|```
rb_equals(rb_build('{1,2,3}'),rb_build('{3,4,5}'))
``` |
|rb\_intersect|roaringbitmap,roaringbitmap|boolean|Checks whether two Roaring bitmaps intersect.|```
rb_intersect(rb_build('{1,2,3}'),rb_build('{3,4,5}'))
``` |
|rb\_remove|roaringbitmap,integer|roaringbitmap|Removes an offset from a Roaring bitmap.|```
rb_remove(rb_build('{1,2,3}'),3)
``` |
|rb\_flip|roaringbitmap,integer,integer|roaringbitmap|Flips specific offsets in a Roaring bitmap.|```
rb_flip(rb_build('{1,2,3}'),2,3)
``` |
|rb\_minimum|roaringbitmap|integer|Returns the smallest offset in a Roaring bitmap. If the Roaring bitmap is empty, the value -1 is returned.|```
rb_minimum(rb_build('{1,2,3}'))
``` |
|rb\_maximum|roaringbitmap|integer|Returns the largest offset in a Roaring bitmap. If the Roaring bitmap is empty, the value 0 is returned.|```
rb_maximum(rb_build('{1,2,3}'))
``` |
|rb\_rank|roaringbitmap,integer|Integer|Returns the number of elements that are smaller than or equal to a specified offset in a Roaring bitmap.|```
rb_rank(rb_build('{1,2,3}'),3)
``` |
|rb\_iterate|roaringbitmap|setof integer|Returns a list of offsets from a Roaring bitmap.|```
rb_iterate(rb_build('{1,2,3}'))
``` |

## Bitmap aggregate functions

|Function|Input data|Message|Description|Examples|
|--------|----------|-------|-----------|--------|
|rb\_build\_agg|Integer|roaringbitmap|Creates a Roaring bitmap from a group of offsets.|```
rb_build_agg(1)
``` |
|rb\_or\_agg|roaringbitmap|roaringbitmap|Performs an OR aggregate operation.|```
rb_or_agg(rb_build('{1,2,3}'))
``` |
|rb\_and\_agg|roaringbitmap|roaringbitmap|Performs an AND aggregate operation.|```
rb_and_agg(rb_build('{1,2,3}'))
``` |
|rb\_xor\_agg|roaringbitmap|roaringbitmap|Performs an XOR aggregate operation.|```
rb_xor_agg(rb_build('{1,2,3}'))
``` |
|rb\_or\_cardinality\_agg|roaringbitmap|integer|Calculates the cardinality from an OR aggregate operation on two Roaring bitmaps.|```
rb_or_cardinality_agg(rb_build('{1,2,3}'))
``` |
|rb\_and\_cardinality\_agg|roaringbitmap|integer|Calculates the cardinality from an AND aggregate operation on two Roaring bitmaps.|```
rb_and_cardinality_agg(rb_build('{1,2,3}'))
``` |
|rb\_xor\_cardinality\_agg|roaringbitmap|integer|Calculates the cardinality from an XOR aggregate operation on two Roaring bitmaps.|```
rb_xor_cardinality_agg(rb_build('{1,2,3}'))
``` |

