# COPY操作是否需要superuser权限 {#concept_brc_tpr_3gb .concept}

## 问题描述 {#section_kq5_5pr_3gb .section}

在AnalyticDB for PostgreSQL上通过执行`CREATE ROLE`创建了一个普通用户gpuser01，但是在执行COPY脚本的时候，提示`ERROR: must be superuser to COPY to or from a file`。

如何操作才能将普通用户权限提升为superuser？

## 问题解答 {#section_m12_fqr_3gb .section}

-   AnalyticDB for PostgreSQL不提供superuser权限，更多信息参考[功能与限制](../../../../cn.zh-CN/产品简介/功能与限制.md#)。
-   可以参考以下两个方法执行数据COPY到文件的操作。

    ```
    psql -c 'copy xx to stdout' > file
    ```

    或

    ```
    
    cat file | psql -c 'copy xx from stdin'
    ```


