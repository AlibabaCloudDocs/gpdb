# 使用压缩位图索引（Roaring Bitmap） {#concept_628336 .concept}

位图（bitmap）是一种常用的数据结构，位图为每个列所有可能的值创建一个单独的位图（0和1的系列），每个位（bit）对应一个数据行是否存在对应的值。通过位图能够快速定位一个数值是否在存在，并利用计算机位级计算的快速特性（AND，OR和ANDNOT等运算），可以显著加快位图的相关计算，非常适合大数据查询和关联计算，常用于去重、标签筛选、时间序列等计算中。

传统的位图会占用大量内存，一般需要对位图进行压缩处理。Roaring Bitmap是一种高效优秀的位图压缩算法，目前已被广泛应用在各种语言和各种大数据平台上。

## Roaring Bitmap压缩算法简介 {#section_n9i_yeu_ydl .section}

Roaring Bitmap的算法是将整数的32-bit的范围 \(\[0, n\]\) 划分为 2^16 个数据块（Chunk），每一个数据块对应整数的高16位，并使用一个容器（Container）来存放一个数值的低16位。 Roaring Bitmap将这些容器保存在一个动态数组中，作为一级索引。容器使用两种不同的结构： 数组容器（Array Container）和 位图容器（Bitmap Container）。数组容器存放稀疏的数据，位图容器存放稠密的数据。如果一个容器里面的整数数量小于4096，就用数组容器来存储值。若大于4096，就用位图容器来存储值。

采用这种存储结构，Roaring Bitmap可以快速检索一个特定的值。在做位图计算（AND，OR，XOR）时，Roaring Bitmap提供了相应的算法来高效地实现在两种容器之间的运算。使得Roaring Bitmap无论在存储和计算性能上都变现优秀。

更多关于Roaring Bitmap的介绍信息，请参考[官方网站](http://roaringbitmap.org/)

## 使用Roaring Bitmap位图计算 {#section_8hb_3pg_ijf .section}

使用Roaring Bitmap位图计算功能之前，首先需要在数据库中创建RoaringBitmap扩展插件：

``` {#codeblock_33q_yvg_ylo}
CREATE EXTENSION roaringbitmap;
```

创建带有RoaringBitmap数据类型的表：

``` {#codeblock_mao_8sl_ezz}
CREATE TABLE t1 (id integer, bitmap roaringbitmap);
```

使用rb\_build函数插入roaringbitmap的数据：

``` {#codeblock_kw8_hty_glp}
--数组位置对应的BIT值为1
INSERT INTO t1 SELECT 1,RB_BUILD(ARRAY[1,2,3,4,5,6,7,8,9,200]);
--将输入的多条记录的值对应位置的BIT值设置为1，最后聚合为一个roaringbitmap  
INSERT INTO t1 SELECT 2,RB_BUILD_AGG(e) FROM GENERATE_SERIES(1,100) e;
```

Bitmap计算\(OR, AND, XOR, ANDNOT\)

``` {#codeblock_d56_c38_5d0}
SELECT RB_OR(a.bitmap,b.bitmap) FROM (SELECT bitmap FROM t1 WHERE id = 1) AS a,(SELECT bitmap FROM t1 WHERE id = 2) AS b;
```

Bitmap聚合计算\(OR, AND, XOR, BUILD\)，并生成新的roaringbitmap类型

``` {#codeblock_6f9_30g_iea}
SELECT RB_OR_AGG(bitmap) FROM t1;
SELECT RB_AND_AGG(bitmap) FROM t1;
SELECT RB_XOR_AGG(bitmap) FROM t1;
SELECT RB_BUILD_AGG(e) FROM GENERATE_SERIES(1,100) e;
```

统计基数（Cardinality）,即统计roaringbitmap中包含多少个位置为1的BIT位

``` {#codeblock_v81_dbi_8pu}
SELECT RB_CARDINALITY(bitmap) FROM t1;
```

从roaringbitmap中返回位置为1的BIT下标（位置值）。

``` {#codeblock_v0c_8fr_ixi}
SELECT RB_ITERATE(bitmap) FROM t1 WHERE id = 1;
```

## Bitmap函数列表 {#section_if3_p1i_1v6 .section}

|函数名|输入|输出|描述|示例|
|---|--|--|--|--|
|rb\_build|integer\[\]|roaringbitmap|通过数组创建一个Bitmap。| ``` {#codeblock_04h_yu1_s53}
rb_build('{1,2,3,4,5}')
```

 |
|rb\_and|roaringbitmap，roaringbitmap|roaringbitmap|And计算。| ``` {#codeblock_3zd_hch_sv0}
rb_and(rb_build('{1,2,3}'),rb_build('{3,4,5}'))
```

 |
|rb\_or|roaringbitmap，roaringbitmap|roaringbitmap|Or计算。| ``` {#codeblock_27s_227_df9}
rb_or(rb_build('{1,2,3}'),rb_build('{3,4,5}'))
```

 |
|rb\_xor|roaringbitmap，roaringbitmap|roaringbitmap|Xor计算。| ``` {#codeblock_sio_12y_0nx}
rb_xor(rb_build('{1,2,3}'),rb_build('{3,4,5}'))
```

 |
|rb\_andnot|roaringbitmap，roaringbitmap|roaringbitmap|AndNot计算。| ``` {#codeblock_30u_lsf_imd}
rb_andnot(rb_build('{1,2,3}'),rb_build('{3,4,5}'))
```

 |
|rb\_cardinality|roaringbitmap|integer|统计基数| ``` {#codeblock_2u6_1bz_hmg}
rb_cardinality(rb_build('{1,2,3,4,5}'))
```

 |
|rb\_and\_cardinality|roaringbitmap，roaringbitmap|integer|And计算并返回基数。| ``` {#codeblock_vop_ajn_hzj}
rb_and_cardinality(rb_build('{1,2,3}'),rb_build('{3,4,5}'))
```

 |
|rb\_or\_cardinality|roaringbitmap，roaringbitmap|integer|Or计算并返回基数。| ``` {#codeblock_j5l_abe_643}
rb_or_cardinality(rb_build('{1,2,3}'),rb_build('{3,4,5}'))
```

 |
|rb\_xor\_cardinality|roaringbitmap，roaringbitmap|integer|Xor计算并返回基数。| ``` {#codeblock_2mv_q50_cyc}
rb_xor_cardinality(rb_build('{1,2,3}'),rb_build('{3,4,5}'))
```

 |
|rb\_andnot\_cardinality|roaringbitmap，roaringbitmap|integer|AndNot计算并返回基数。| ``` {#codeblock_9n6_r25_yge}
rb_andnot_cardinality(rb_build('{1,2,3}'),rb_build('{3,4,5}'))
```

 |
|rb\_is\_empty|roaringbitmap|boolean|判断是否为空的Bitmap。| ``` {#codeblock_2ba_kcb_r6r}
rb_is_empty(rb_build('{1,2,3,4,5}'))
```

 |
|rb\_equals|roaringbitmap，roaringbitmap|boolean|判断两个Bitmap是否相等。| ``` {#codeblock_hos_d6a_shd}
rb_equals(rb_build('{1,2,3}'),rb_build('{3,4,5}'))
```

 |
|rb\_intersect|roaringbitmap，roaringbitmap|boolean|判断两个Bitmap是否相交。| ``` {#codeblock_o36_8r0_1e5}
rb_intersect(rb_build('{1,2,3}'),rb_build('{3,4,5}'))
```

 |
|rb\_remove|roaringbitmap,integer|roaringbitmap|从Bitmap移除特定的Offset。| ``` {#codeblock_1mo_vlq_dju}
rb_remove(rb_build('{1,2,3}'),3)
```

 |
|rb\_flip|roaringbitmap,integer,integer|roaringbitmap|翻转Bitmap中特定的Offset段。| ``` {#codeblock_ubn_0qr_pib}
rb_flip(rb_build('{1,2,3}'),2,3)
```

 |
|rb\_minimum|roaringbitmap|integer|返回Bitmap中最小的Offset，如果Bitmap为空则返回-1。| ``` {#codeblock_1wx_9ga_ldr}
rb_minimum(rb_build('{1,2,3}'))
```

 |
|rb\_maximum|roaringbitmap|integer|返回Bitmap中最大的Offset，如果Bitmap为空则返回0。| ``` {#codeblock_aeb_ajd_v5g}
rb_maximum(rb_build('{1,2,3}'))
```

 |
|rb\_rank|roaringbitmap,integer|integer|返回Bitmap中小于等于指定Offset的基数。| ``` {#codeblock_t11_pzc_l4t}
rb_rank(rb_build('{1,2,3}'),3)
```

 |
|rb\_iterate|roaringbitmap|setof integer|返回Offset List。| ``` {#codeblock_zb1_uhl_o7o}
rb_iterate(rb_build('{1,2,3}'))
```

 |

## Bitmap聚合函数列表 {#section_6zl_8ip_shz .section}

|聚合函数名|输入|输出|描述|示例|
|-----|--|--|--|--|
|rb\_build\_agg|integer|roaringbitmap|将Offset聚合成bitmap。| ``` {#codeblock_wo4_ytn_w95}
rb_build_agg(1)
```

 |
|rb\_or\_agg|roaringbitmap|roaringbitmap|Or 聚合计算。| ``` {#codeblock_bt1_jvg_tzv}
rb_or_agg(rb_build('{1,2,3}'))
```

 |
|rb\_and\_agg|roaringbitmap|roaringbitmap|And 聚合计算。| ``` {#codeblock_4b8_bcg_1iw}
rb_and_agg(rb_build('{1,2,3}'))
```

 |
|rb\_xor\_agg|roaringbitmap|roaringbitmap|Xor 聚合计算。| ``` {#codeblock_pko_ikm_lm2}
rb_xor_agg(rb_build('{1,2,3}'))
```

 |
|rb\_or\_cardinality\_agg|roaringbitmap|integer|Or 聚合计算并返回其基数。| ``` {#codeblock_9zb_psm_lvo}
rb_or_cardinality_agg(rb_build('{1,2,3}'))
```

 |
|rb\_and\_cardinality\_agg|roaringbitmap|integer|And 聚合计算并返回其基数。| ``` {#codeblock_lio_ni3_sjg}
rb_and_cardinality_agg(rb_build('{1,2,3}'))
```

 |
|rb\_xor\_cardinality\_agg|roaringbitmap|integer|Xor 聚合计算并返回其基数。| ``` {#codeblock_5cq_z2o_95z}
rb_xor_cardinality_agg(rb_build('{1,2,3}'))
```

 |

