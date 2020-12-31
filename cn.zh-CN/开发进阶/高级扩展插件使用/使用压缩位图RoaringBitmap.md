# 使用压缩位图RoaringBitmap

位图（bitmap）是一种常用的数据结构，位图为每个列所有可能的值创建一个单独的位图（0和1的系列），每个位（bit）对应一个数据行是否存在对应的值。通过位图能够快速定位一个数值是否在存在，并利用计算机位级计算的快速特性（AND，OR和ANDNOT等运算），可以显著加快位图的相关计算，非常适合大数据查询和关联计算，常用于去重、标签筛选、时间序列等计算中。

传统的位图会占用大量内存，一般需要对位图进行压缩处理。Roaring Bitmap是一种高效优秀的位图压缩算法，目前已被广泛应用在各种语言和各种大数据平台上。

## Roaring Bitmap压缩算法简介

Roaring Bitmap的算法是将整数的32-bit的范围 \(\[0, n\]\) 划分为 2^16 个数据块（Chunk），每一个数据块对应整数的高16位，并使用一个容器（Container）来存放一个数值的低16位。 Roaring Bitmap将这些容器保存在一个动态数组中，作为一级索引。容器使用两种不同的结构： 数组容器（Array Container）和 位图容器（Bitmap Container）。数组容器存放稀疏的数据，位图容器存放稠密的数据。如果一个容器里面的整数数量小于4096，就用数组容器来存储值。若大于4096，就用位图容器来存储值。

采用这种存储结构，Roaring Bitmap可以快速检索一个特定的值。在做位图计算（AND，OR，XOR）时，Roaring Bitmap提供了相应的算法来高效地实现在两种容器之间的运算。使得Roaring Bitmap无论在存储和计算性能上都变得优秀。

更多关于Roaring Bitmap的介绍信息，请参考[官方网站](https://github.com/RoaringBitmap/CRoaring)

## 使用Roaring Bitmap位图计算

使用Roaring Bitmap位图计算功能之前，首先需要在数据库中创建RoaringBitmap扩展插件：

```
CREATE EXTENSION roaringbitmap;
```

创建带有RoaringBitmap数据类型的表：

```
CREATE TABLE t1 (id integer, bitmap roaringbitmap);
```

使用rb\_build函数插入roaringbitmap的数据：

```
--数组位置对应的BIT值为1
INSERT INTO t1 SELECT 1,RB_BUILD(ARRAY[1,2,3,4,5,6,7,8,9,200]);
--将输入的多条记录的值对应位置的BIT值设置为1，最后聚合为一个roaringbitmap  
INSERT INTO t1 SELECT 2,RB_BUILD_AGG(e) FROM GENERATE_SERIES(1,100) e;
```

Bitmap计算\(OR, AND, XOR, ANDNOT\)

```
SELECT RB_OR(a.bitmap,b.bitmap) FROM (SELECT bitmap FROM t1 WHERE id = 1) AS a,(SELECT bitmap FROM t1 WHERE id = 2) AS b;
```

Bitmap聚合计算\(OR, AND, XOR, BUILD\)，并生成新的roaringbitmap类型

```
SELECT RB_OR_AGG(bitmap) FROM t1;
SELECT RB_AND_AGG(bitmap) FROM t1;
SELECT RB_XOR_AGG(bitmap) FROM t1;
SELECT RB_BUILD_AGG(e) FROM GENERATE_SERIES(1,100) e;
```

统计基数（Cardinality）,即统计roaringbitmap中包含多少个位置为1的BIT位

```
SELECT RB_CARDINALITY(bitmap) FROM t1;
```

从roaringbitmap中返回位置为1的BIT下标（位置值）。

```
SELECT RB_ITERATE(bitmap) FROM t1 WHERE id = 1;
```

## Bitmap函数列表

|函数名|输入|输出|描述|示例|
|---|--|--|--|--|
|rb\_build|integer\[\]|roaringbitmap|通过数组创建一个Bitmap。|```
rb_build('{1,2,3,4,5}')
``` |
|rb\_and|roaringbitmap，roaringbitmap|roaringbitmap|And计算。|```
rb_and(rb_build('{1,2,3}'),rb_build('{3,4,5}'))
``` |
|rb\_or|roaringbitmap，roaringbitmap|roaringbitmap|Or计算。|```
rb_or(rb_build('{1,2,3}'),rb_build('{3,4,5}'))
``` |
|rb\_xor|roaringbitmap，roaringbitmap|roaringbitmap|Xor计算。|```
rb_xor(rb_build('{1,2,3}'),rb_build('{3,4,5}'))
``` |
|rb\_andnot|roaringbitmap，roaringbitmap|roaringbitmap|AndNot计算。|```
rb_andnot(rb_build('{1,2,3}'),rb_build('{3,4,5}'))
``` |
|rb\_cardinality|roaringbitmap|integer|统计基数|```
rb_cardinality(rb_build('{1,2,3,4,5}'))
``` |
|rb\_and\_cardinality|roaringbitmap，roaringbitmap|integer|And计算并返回基数。|```
rb_and_cardinality(rb_build('{1,2,3}'),rb_build('{3,4,5}'))
``` |
|rb\_or\_cardinality|roaringbitmap，roaringbitmap|integer|Or计算并返回基数。|```
rb_or_cardinality(rb_build('{1,2,3}'),rb_build('{3,4,5}'))
``` |
|rb\_xor\_cardinality|roaringbitmap，roaringbitmap|integer|Xor计算并返回基数。|```
rb_xor_cardinality(rb_build('{1,2,3}'),rb_build('{3,4,5}'))
``` |
|rb\_andnot\_cardinality|roaringbitmap，roaringbitmap|integer|AndNot计算并返回基数。|```
rb_andnot_cardinality(rb_build('{1,2,3}'),rb_build('{3,4,5}'))
``` |
|rb\_is\_empty|roaringbitmap|boolean|判断是否为空的Bitmap。|```
rb_is_empty(rb_build('{1,2,3,4,5}'))
``` |
|rb\_equals|roaringbitmap，roaringbitmap|boolean|判断两个Bitmap是否相等。|```
rb_equals(rb_build('{1,2,3}'),rb_build('{3,4,5}'))
``` |
|rb\_intersect|roaringbitmap，roaringbitmap|boolean|判断两个Bitmap是否相交。|```
rb_intersect(rb_build('{1,2,3}'),rb_build('{3,4,5}'))
``` |
|rb\_remove|roaringbitmap,integer|roaringbitmap|从Bitmap移除特定的Offset。|```
rb_remove(rb_build('{1,2,3}'),3)
``` |
|rb\_flip|roaringbitmap,integer,integer|roaringbitmap|翻转Bitmap中特定的Offset段。|```
rb_flip(rb_build('{1,2,3}'),2,3)
``` |
|rb\_minimum|roaringbitmap|integer|返回Bitmap中最小的Offset，如果Bitmap为空则返回-1。|```
rb_minimum(rb_build('{1,2,3}'))
``` |
|rb\_maximum|roaringbitmap|integer|返回Bitmap中最大的Offset，如果Bitmap为空则返回0。|```
rb_maximum(rb_build('{1,2,3}'))
``` |
|rb\_rank|roaringbitmap,integer|integer|返回Bitmap中小于等于指定Offset的基数。|```
rb_rank(rb_build('{1,2,3}'),3)
``` |
|rb\_iterate|roaringbitmap|setof integer|返回Offset List。|```
rb_iterate(rb_build('{1,2,3}'))
``` |
|rb\_contains|roaringbitmap, integer|bool|判断Bitmap是否包含特定的Offset|```
rb_contains(rb_build('{1,2,3}'),1)
``` |
|rb\_contains|roaringbitmap, integer, integer|bool|判断Bitmap是否包含特定的Offset段（某个范围）|```
rb_contains(rb_build('{1,2,3}'),rb_build('{3,4,5}'))
``` |
|rb\_contains|roaringbitmap, roaringbitmap|bool|判断Bitmap是否包含另外一个bitmap|```
 rb_contains(rb_build('{1,2,3}'),rb_build('{3,4,5}'))
``` |
|rb\_becontained|integer, roaringbitmap|bool|判断特定的Offset是否被Bitmap包含|```
rb_becontained(1, rb_build('{1,2,3}'))
``` |
|rb\_becontained|roaringbitmap, roaringbitmap|bool|判断Bitmap是否被另外一个包含。|```
rb_becontained(rb_build('{1}'), rb_build('{1,2,3}'))
``` |
|rb\_add|roaringbitmap, integer|roaringbitmap|添加特定的Offset到Bitma|```
rb_add(rb_build('{1,2,3,4}'), 5)
``` |
|rb\_add\_2|integer, roaringbitmap|roaringbitmap|Add a specific offset to roaringbitmap.|```
rb_add_2(5, rb_build('{1,2,3,4}'))
``` |
|rb\_add|roaringbitmap, integer, integer|roaringbitmap|添加特定的Offset段到Bitmap|```
rb_add(rb_build('{1,2,3,4}'), 6,8)
``` |
|rb\_remove|roaringbitmap, integer, integer|roaringbitmap|从Bitmap移除特定的Offset|```
rb_remove(rb_build('{1,2,3,4,6,7,8}'), 6,8)
``` |
|rb\_jaccard\_index|roaringbitmap, roaringbitmap|float8|计算两个Bitmap之间的jaccard相似系数|```
rb_jaccard_index(rb_build('{1,2,3,4}'), rb_build('{1,2}'))
``` |
|rb\_to\_array|roaringbitmap|integer\[\]|Bitmap转数组|```
rb_to_array(rb_build('{1,2,3,4}'))
``` |
|rb\_iterate\_decrement|roaringbitmap|integer\[\]|返回Offset List（从大到小）|```
rb_iterate_decrement(rb_build('{1,2,3,4}'))
``` |

## Bitmap聚合函数列表

|聚合函数名|输入|输出|描述|示例|
|-----|--|--|--|--|
|rb\_build\_agg|integer|roaringbitmap|将Offset聚合成bitmap。|```
rb_build_agg(1)
``` |
|rb\_or\_agg|roaringbitmap|roaringbitmap|Or 聚合计算。|```
rb_or_agg(rb_build('{1,2,3}'))
``` |
|rb\_and\_agg|roaringbitmap|roaringbitmap|And 聚合计算。|```
rb_and_agg(rb_build('{1,2,3}'))
``` |
|rb\_xor\_agg|roaringbitmap|roaringbitmap|Xor 聚合计算。|```
rb_xor_agg(rb_build('{1,2,3}'))
``` |
|rb\_or\_cardinality\_agg|roaringbitmap|integer|Or 聚合计算并返回其基数。|```
rb_or_cardinality_agg(rb_build('{1,2,3}'))
``` |
|rb\_and\_cardinality\_agg|roaringbitmap|integer|And 聚合计算并返回其基数。|```
rb_and_cardinality_agg(rb_build('{1,2,3}'))
``` |
|rb\_xor\_cardinality\_agg|roaringbitmap|integer|Xor 聚合计算并返回其基数。|```
rb_xor_cardinality_agg(rb_build('{1,2,3}'))
``` |

## 操作符

|操作符|left|right|output|描述|示例|
|---|----|-----|------|--|--|
|&|roaringbitmap|roaringbitmap|roaringbitmap|两个Bitmap And 操作|```
rb_build('{1,2,3}') & rb_build('{1,2,4}')
``` |
|\||roaringbitmap|roaringbitmap|roaringbitmap|两个Bitmap Or 操作|```
rb_build('{1,2}') | rb_build('{1,3}')
``` |
|\#|roaringbitmap|roaringbitmap|roaringbitmap|两个Bitmap Xor 操作|```
rb_build('{1,2}') # rb_build('{1,3}')
``` |
|~|roaringbitmap|roaringbitmap|roaringbitmap|两个Bitmap Andnot 操作|```
rb_build('{2,3}') ~ rb_build('{2,4}')
``` |
|+|roraingbitmap|integer|roaringbitmap|向Bitmap中添加特定的Offset|```
rb_build('{2,3}') + 1
``` |
|-|roraingbitmap|integer|roaringbitmap|从Bitmap移除特定的Offset|```
rb_build('{1,2,3}') - 1
``` |
|=|roaringbitmap|roaringbitmap|boolean|判断两个Bitmap是否相等|```
rb_build('{2,3}') = rb_build('{2,3}')
``` |
|<\>|roaringbitmap|roaringbitmap|boolean|判断两个Bitmap是否不相等|```
rb_build('{2,3}') <> rb_build('{1,2,3}')
``` |
|&&|roaringbitmap|roaringbitmap|boolean|判断两个Bitmap是否相交|```
rb_build('{2,3}') && rb_build('{3,4}')
``` |
|@\>|roaringbitmap|roaringbitmap|boolean|判断Bitmap是否包含另外一个|```
rb_build('{2,3}') @> rb_build('{2}')
``` |
|@\>|roaringbitmap|integer|boolean|判断Bitmap是否包含特定的Offset|```
rb_build('{2,3}') @> 2
``` |
|<@|roaringbitmap|roaringbitmap|boolean|判断Bitmap是否被另外一个包含|```
rb_build('{2,3}') <@ rb_build('{1,2,3}')
``` |
|<@|integer|roaringbitmap|boolean|判断特定的Offset是否被Bitmap包含|```
rb_build('{2,3}') <@ rb_build('{3}')
``` |

