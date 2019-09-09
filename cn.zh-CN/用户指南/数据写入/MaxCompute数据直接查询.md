# MaxCompute数据直接查询 {#concept_1563685 .concept}

交互式分析（Interactive Analytics）与大数据生态无缝打通，支持以PostgreSQL协议及语法直接访问MaxCompute项目并进行分析，快速获取查询结果，节省了大量的中间转换时间，无冗余存储，也无数据集成开发维护成本。

针对不用的业务特性和场景，您可采用以下2种方式来进行MaxCompute数据的交互式分析 ：

-   **MaxCompute直接分析**：首选方案，通过交互式分析（Interactive Analytics）直接分析MaxCompute数据即可， 无冗余存储，无数据导入导出操作。
-   **MaxCompute数据index build**：交互式分析（Interactive Analytics）与MaxCompute底层无缝打通，支持盘古存储文件直接读取，方便您按照传统OLAP的方式，将数据导入交互式分析（Interactive Analytics）构建索引，适用于超大规模（MaxCompute表单分区大于1TB）、查询复杂的场景。相比其他非大数据生态产品，该方法的数据导入导出速度要更快。

本小节将会为您介绍在交互式分析（Interactive Analytics）中如何直接加速查询MaxCompute表数据。

## 示例1：MaxCompute非分区表数据查询 {#section_mvp_ug2_clq .section}

1.  **准备MaxCompute非分区表数据** 

    您可以参考[MaxCompute快速入门](https://help.aliyun.com/document_detail/27808.html?spm=a2c4g.11186623.6.589.613b2b17BCOZWs)，创建表并导入数据，也可以直接从数据地图搜索已有MaxCompute表。示例选用数据地图中的一张表，其DDL如下：

    ``` {#codeblock_kbv_b2r_0j5 .lanuage-sql}
    CREATE TABLE weather (
        city            STRING ,
        temp_lo         int,           -- 最低温度
        temp_hi         int         -- 最高温度
    );
    INSERT INTO weather VALUES 
    ('beijing',40,50),
    ('hangzhou',46,55);
    ```

2.  **交互式分析（Interactive Analytics）创建外部表** 

    这一步的作用是建立映射表，目的是让交互式分析（Interactive Analytics）读取MaxCompute源头表数据。外部表语义和参数完全兼容标准pg，详情参考[外部表](cn.zh-CN/用户指南/SQL参考/DDL/外部表.md#)。示例建表SQL如下：

    ``` {#codeblock_nwv_xl5_r35 .lanuage-sql}
    CREATE FOREIGN TABLE weather1 (
     city text,
     temp_lo int8,
     temp_hi int8
    )
    SERVER odps_server
    OPTIONS (project_name 'dw_test_010',table_name 'weather');
    ```

    **说明：** ：

    -   字段名必须与MaxCompute字段名保持一致。
    -   映射字段可以根据需要全部映射，或者只是映射部分。
    -   options参数给定MaxCompute的project和table。
    -   当MaxCompute project下表的schema发生变化时，交互式分析（Interactive Analytics）当前版本不会做schema的自动更新，由您自己负责更新，详细的命令参考[PostgreSQL官网](https://www.postgresql.org/docs/11/sql-alterforeigntable.html)。
    -   当前版本不支持import foreign schema。
3.  **查询外部表** 

    外部表创建成功后，直接查询外部表，即可查询到MaxCompute的数据

    ``` {#codeblock_lf9_4kc_t1y .lanuage-sql}
    select * from weather1；
    ```


## 示例2：MaxCompute分区表数据直接查询 {#section_sh9_12m_8te .section}

1.  **准备Maxcompute分区表数据** 

    准备一张MaxCompute分区数据表。示例选用数据地图已有的分区表，其DDL如下：

    ``` {#codeblock_6z2_7qx_xra .lanuage-sql}
    create table odps_test
    (
        shop_name     string,
        customer_id   string,
        total_price   INT 
    )
    partitioned by (sale_date string);
    ```

2.  **交互式分析（Interactive Analytics）创建外部表** 

    在交互式分析（Interactive Analytics）中创建一张外部表，用于映射MaxCompute中源头数据表。示例外部表的DDL如下。

    ``` {#codeblock_4ci_w77_ws5 .lanuage-sql}
    CREATE FOREIGN TABLE table_odps (
     shop_name text,
     customer_id text,
     total_price int8,
     sale_date text
    )
    SERVER odps_server
    OPTIONS (project_name 'hologres_poc_dev', table_name 'odps_test');
    ```

3.  **查询分区表** 

    查询整张表，示例SQL如下。

    ``` {#codeblock_n02_t3o_j93 .lanuage-sql}
    SELECT * FROM table_odps;
    ```

    查询分区数据，示例SQL如下。

    ``` {#codeblock_e9e_5q5_dbd .lanuage-sql}
    SELECT * FROM table_odps 
    WHERE sale_date = '2013';
    ```


