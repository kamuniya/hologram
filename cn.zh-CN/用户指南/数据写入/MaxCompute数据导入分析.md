# MaxCompute数据导入分析 {#concept_2071155 .concept}

针对部分业务数据规模大（大于1TB）、查询query复杂度高且查询要求秒级响应的场景，交互式分析（Interactive Analytics）支持MaxCompute数据index build，相比其他工具需要数据导入导出节省了大量中间转化成本。本小节将会为您介绍如何将MaxCompute中的数据导入至交互式分析（Interactive Analytics）中分析。

数据从MaxCompute导入到交互式分析（Interactive Analytics）中需要如下四个步骤：

1.  准备MaxCompute中的源头数据表。
2.  在交互式分析（Interactive Analytics）中建立一张外映射MaxCompute源头数据表的外部表。
3.  在交互式分析（Interactive Analytics）中建立接收数据的真实存储表。
4.  调用Insert \[overwrite\] into select来完成数据导入。

**说明：** ：MaxCompute是否分区与交互式分析（Interactive Analytics）是否分区无强关联。

-   MaxCompute分区表可以全部insert into一张交互式分析（Interactive Analytics）非分区表。
-   MaxCompute分区表也可以insert into一张交互式分析（Interactive Analytics）分区表，分区之间一一映射。
-   交互式分析（Interactive Analytics）暂时不支持多级分区，所以MaxCompute多级分区到交互式分析（Interactive Analytics）改成非分区或者一级分区即可。

## 示例1：MaxCompute非分区数据导入查询 {#section_8b6_suw_lbb .section}

1.  **准备MaxCompute非分区表数据** 

    在MaxCopmpute中创建一张非分区源头数据表，也可以直接选用数据地图中的一张非分区表。示例选数据地图中的一张表，其DDL如下。

    ``` {#codeblock_wef_r3s_hb2 .lanuage-sql}
    CREATE TABLE odps_test1(
        id int,
        name STRING ,
        class STRING 
    );
    ```

2.  **交互式分析（Interactive Analytics）中创建一张外部表** 

    在交互式分析（Interactive Analytics）中创建一张外部表，用于映射MaxCompute中的源头数据表。

    ``` {#codeblock_chc_z2b_1s1 .lanuage-sql}
    CREATE FOREIGN TABLE holo_test1 (
     id int8,
     name text,
     class text
    )
    SERVER odps_server
    OPTIONS (project_name 'hologres_poc_dev', table_name 'odps_test1');
    ```

3.  **交互式分析（Interactive Analytics）建立一张真实存储表** 

    在交互式分析（Interactive Analytics）中建立一张真实存储表，用于接收MaxCompute源头表数据，示例DDL如下。

    ``` {#codeblock_3az_eui_63f .lanuage-sql}
    CREATE TABLE holo_test2 (
     id int8,
     name text,
     class text
    );
    ```

4.  **导入数据至交互式分析（Interactive Analytics）** 

    将MaxCompute源头表中的数据导入到交互式分析（Interactive Analytics）表中，示例DDL如下。

    ``` {#codeblock_idp_cs6_zvx .lanuage-sql}
    INSERT INTO holo_test2
    SELECT 
    id as id,
    name as name,
    class as class
    FROM holo_test1;
    ```

5.  **交互式分析（Interactive Analytics）查询MaxCompute表数据** 

    输入查询语句，即可在交互式分析（Interactive Analytics）中查询MaxCompute表中的数据，示例如下。

    ``` {#codeblock_uf1_gy6_fgx .lanuage-sql}
    SELECT * FROM holo_test2;
    ```


## 示例2：MaxCompute分区数据导入查询 {#section_jgn_gme_1ih .section}

1.  **准备MaxCompute分区数据表** 

    在MaxCompute中准备一张分区数据表，示例选用数据地图中已有的一张分区表，其DDL如下：

    ``` {#codeblock_mts_qmo_xd7 .lanuage-sql}
    create table odps_test2
    (
        shop_name     string,
        customer_id   string,
        total_price   INT 
    )
    partitioned by (sale_date string);
    ```

2.  **交互式分析（Interactive Analytics）建立一张外部表** 

    在交互式分析（Interactive Analytics）中建立一张外部表，用于映射MaxCompute源头数据表，示例DDL如下。

    ``` {#codeblock_7ie_x3k_70i .lanuage-sql}
    CREATE FOREIGN TABLE table1_odps (
     shop_name text,
     customer_id text,
     total_price int8,
     sale_date text
    )
    SERVER odps_server
    OPTIONS (project_name 'hologres_poc_dev', table_name 'odps_test2');
    ```

3.  **交互式分析（Interactive Analytics）建立一张真实存储表** 

    在交互式分析（Interactive Analytics）中建立一张真实存储数据的表，用于存储MaxCompute源头数据表中导入的数据，示例DDL如下。

    ``` {#codeblock_2z5_bz8_kxe .lanuage-sql}
    CREATE TABLE table1_holo (
     shop_name text,
     customer_id text,
     total_price int8,
     sale_date text
    )
    PARTITION BY LIST (sale_date);
    ```

4.  **创建子分区** 

    在交互式分析（Interactive Analytics）中创建一张子分区表，将分区数据同步，示例DDL如下。

    ``` {#codeblock_tor_mu4_ur8 .lanuage-sql}
    CREATE TABLE  table1_holo_0001
    PARTITION  of table1_holo
    for VALUES  in ('2015');
    ```

5.  **导入数据至交互式分析（Interactive Analytics）** 

    将MaxCompute中的数据导入至交互式分析（Interactive Analytics）的子分区表中，示例DDL如下：

    ``` {#codeblock_nkg_42l_o44}
    insert into table1_holo_0001
    select 
     shop_name as shop_name,
     customer_id as customer_id,
     total_price as total_price,
     sale_date as sale_date
     from table1_odps
     WHERE sale_date = '2015';
    ```

6.  **查询数据** 

    在交互式分析（Interactive Analytics）中查询导入的数据，示例DDL如下。

    ``` {#codeblock_5fk_t4v_a7c}
    SELECT * FROM table1_holo;
    ```


