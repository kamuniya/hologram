---
keyword: [优化性能, MaxCompute外部表, 最佳实践]
---

# 优化MaxCompute外部表的查询性能

本文为您介绍在Hologres中如何优化查询MaxCompute外部表数据的性能。

Hologres与MaxCompute在底层资源无缝打通，您可以通过以下方式加速查询MaxCompute的数据：

-   新建外部表。

    在Hologres中新建外部表，无需导入导出即可查询外部表数据。该方式适用于单次查询的数据量小于100 GB的表。

-   导入外部表数据。

    当需要大量分析计算外部表数据并建立与内部表的连接时，您可以在Hologres中新建内部表并导入外部表数据。根据业务需求，为内部表指定合适的Distribute Key索引属性，加快查询速度。

    导入外部表数据相比新建外部表方式查询速度更快。该方式适用于单次查询的数据量大于等于100 GB的表，以及使用复杂查询、包含索引查询、更新数据或插入数据的场景。

    导入MaxCompute外部表数据至Hologres的操作请参见[t1644102.md\#](/cn.zh-CN/数据接入/大数据/MaxCompute/使用SQL导入MaxCompute的数据至Hologres.md)。


您还可以根据实际业务需求，通过优化查询语句、修改MaxCompute数据源表、合理配置资源和参数，优化查询外部表数据的性能。

## 优化查询语句

使用如下方式优化查询语句，避免查询外部表数据时扫描全表：

-   查询数据时使用`select a from xx`语句查询指定内容。
-   增加过滤分区的条件或减少扫描的分区数，实现减少扫描的数据量。

## 修改MaxCompute数据源表

通过修改MaxCompute数据源表优化查询数据的性能，方式如下：

-   转换MaxCompute表为Hash Clustering表。

    Hash Clustering表可以优化Bucket Pruning、Aggregation以及存储。

    创建MaxCompute表时，如果使用`Clustered By`指定了Hash Key，则MaxCompute对指定列进行Hash运算，并分散Hash值至各个Bucket中。如果没有指定Hash Key，则使用如下语句指定。

    ```
    ALTER TABLE table_name [CLUSTERED BY (col_name [, col_name, ...]) [SORTED BY (col_name [ASC | DESC] [, col_name [ASC | DESC] ...])] INTO number_of_buckets BUCKETS]
    ```

    请选择重复键值少的列作为Hash Key。

    `ALTER TABLE`语句适用于指定存量表的Hash Key。

    新增聚集属性后，新的分区数据写入MaxCompute时直接执行Hash Clustering计算。同时，您可以使用`INSERT OVERWRITE`语句对原有的源表数据执行Hash Clustering计算。

    **说明：**

    -   Hash Clustering表不支持`INSERT INTO`语句，您需要使用`INSERT OVERWRITE`语句添加数据。
    -   Hash Clustering表不支持通过Tunnel方式上传数据至Range Cluster表。
-   减少小文件数量。

    当MaxCompute中的小文件数量较多时，会降低查询表数据的速度。您可以使用如下方式避免产生小文件：

    -   减少计算过程中产生的小文件。

        使用`INSERT OVERWRITE`语句更新数据至源表，或者创建新表，添加数据至新表后删除源表。

    -   减少Tunnel数据采集过程中产生的小文件。
        -   调用Tunnel SDK时，每当Buffer达到64 MB时，提交一次。
        -   使用客户端时避免频繁上传小文件，建议积累至一定数量后一次性上传。
        -   如果导入数据至分区表，建议为分区表设置生命周期，到期时系统会自动清理过期数据。
        -   使用`INSERT OVERWRITE`语句添加数据至源表或分区表。
        -   通过客户端命令执行`ALTER`合并操作。
    -   创建临时表时设置生命周期，到期后垃圾回收机制会自动回收临时表。
    -   使用如下策略合理申请DataHub Shard，减少产生小文件。
        -   根据单个Shard的默认吞吐量分配Shard数目，默认吞吐量为1 MB/s。
        -   当按照小时划分分区时，每个Shard每个小时会产生12个文件。如果此时数据量很少而Shard很多，则MaxCompute产生小文件的个数为`Shard*12/h`。

            MaxCompute的每个Shard拥有一个单独的任务，您可以设置每隔5 min或者每当Buffer达到64 MB时，提交一次任务，提升查询MaxCompute数据的速度。

        -   根据实际业务需求分配Shard。

## 合理配置资源

成功购买Hologres实例后，系统会按照实例规格分配资源。您可以联系技术支持或[提交工单](https://selfservice.console.aliyun.com/ticket/createIndex?spm=5176.2020520129.console-base-top.dwork-order-1.29d546aee0gsiH)，根据业务需求合理配置资源。

## 合理配置参数

通过合理配置如下参数，优化查询外部表数据的性能。

```
SET hg_experimental_query_batch_size = xx  //设置每次读取MaxCompute表Batch的大小，默认为8192个。
SET hg_experimental_foreign_table_split_size = xx  //设置MaxCompute表并发访问切分Spilit的数目。
```

