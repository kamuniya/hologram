# MaxCompute数据直接加速查询 {#concept_1681314 .concept}

交互式分析（Interactive Analytics）支持直接访问MaxCompute项目并对其数据直接加速查询，快速获取结果。本小节将会为您介绍在交互式分析（Interactive Analytics）中如何通过psql客户端对MaxCompute数据直接加速查询。

1.  **准备MaxCompute源头数据表** 

    您可以参见[MaxCompute创建和查看表](https://help.aliyun.com/document_detail/27808.html?spm=a2c4g.11186623.6.589.613b2b17lT7Dsr)，进行建表操作，也可以直接从**DataWorks** \> **数据地图**搜索已有MaxCompute表。

    示例直接选用MaxCompute已有表，其DDL如下。

    ``` {#codeblock_806_ioc_887 .lanuage-sql}
    CREATE TABLE `lightning_cat_industry` (
      `ind_type` STRING,
      `type_id` BIGINT ,
      `ind1_id` BIGINT ,
      `ind1_name` STRING ,
      `cate_level1_id` BIGINT ,
      `cate_level1_name` STRING ,
      `xcat1_id` BIGINT  ,
      `xcat1_name` STRING ,
      `cate_id` BIGINT  ,
      `cate_name` STRING  ,
      `cate_level2_id` BIGINT,
      `cate_level2_name` STRING 
    )
    PARTITIONED BY (
      ds STRING COMMENT '分区字段YYYYMMDD'
    )
    LIFECYCLE 7;
    ```

2.  **创建外部表** 

    这一步是建立映射表，用于交互式分析（Interactive Analytics）读取MaxCompute源头表数据。外部表语义和参数完全兼容标准PostgresSQL，详情参见[外部表](../../../../cn.zh-CN/用户指南/SQL参考/DDL/外部表.md#)。示例SQL命令语句如下。

    ``` {#codeblock_0im_xi6_747 .lanuage-sql}
    create foreign table
     lightning_cat_industry_sc(
      ind_type text ,
      type_id int ,
      ind1_id int ,
      ind1_name text ,
      cate_level1_id int ,
      cate_level1_name text ,
      xcat1_id int ,
      xcat1_name text ,
      cate_id int ,
      cate_name text ,
      cate_level2_id int,
      cate_level2_name text,
      ds text
    ) server odps_server options(project_name 'lightning_compute', table_name 'lightning_cat_industry');
    ```

    **说明：** ：

    -   字段名必须与MaxCompute字段名保持一致。
    -   映射字段可以根据需要全部映射，或者只是映射部分。
    -   options参数给定MaxCompute的project和table。
    -   当MaxCompute project下表的schema发生变化时，交互式分析（Interactive Analytics）不会做schema的自动更新，由用户自己负责更新，详细的命令参见[文档](https://www.postgresql.org/docs/11/sql-alterforeigntable.html)。
    -   不支持import foreign schema。
3.  **查询外部表** 

    外部表创建成功后，输入以下SQL命令语句即可查询MaxCompute的表数据。

    ``` {#codeblock_5j2_u63_flz .lanuage-sql}
    select count(*) from lightning_cat_industry_demo2 where ds='20190720';
    select * from lightning_cat_industry_demo2 limit 10;
    ```


