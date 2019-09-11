# 示例：HoloStudio周期性调度 {#concept_2071431 .concept}

本小节将会为您介绍如何将MaxCompute源头表数据导入交互式分析（Interactive Analytics）中进行周期性调度。

1.  **准备MaxCompute表数据** 

    准备一张MaxCompute源头数据表，您可以参考[MaxCompute快速入门](https://help.aliyun.com/document_detail/27808.html?spm=a2c4g.11186623.6.589.613b2b17XHk7rR)，创建一张表并导入数据，也可以直接从数据地图中选用一张表。示例选用数据地图中的已有表，其DDL如下。

    ``` {#codeblock_kfm_y31_52x .lanuage-sql}
    CREATE TABLE IF NOT EXISTS bank_data_odps
    (
     age             BIGINT COMMENT '年龄',
     job             STRING COMMENT '工作类型',
     marital         STRING COMMENT '婚否',
     education       STRING COMMENT '教育程度',
     card         STRING COMMENT '是否有信用卡',
     housing         STRING COMMENT '房贷',
     loan            STRING COMMENT '贷款',
     contact         STRING COMMENT '联系途径',
     month           STRING COMMENT '月份',
     day_of_week     STRING COMMENT '星期几',
     duration        STRING COMMENT '持续时间',
     campaign        BIGINT COMMENT '本次活动联系的次数',
     pdays           DOUBLE COMMENT '与上一次联系的时间间隔',
     previous        DOUBLE COMMENT '之前与客户联系的次数',
     poutcome        STRING COMMENT '之前市场活动的结果',
     emp_var_rate    DOUBLE COMMENT '就业变化速率',
     cons_price_idx  DOUBLE COMMENT '消费者物价指数',
     cons_conf_idx   DOUBLE COMMENT '消费者信心指数',
     euribor3m       DOUBLE COMMENT '欧元存款利率',
     nr_employed     DOUBLE COMMENT '职工人数',
     y               BIGINT COMMENT '是否有定期存款'
    );
    ```

2.  **HoloStudio新建外部表数据开发** 

    进入HoloStudio，单击左侧菜单栏中**数据开发** \> **新建数据开发**，新建一张外部表，用于映射MaxCompute源头表数据。

    示例SQL命令如下，并单击**保存**，然后单击左上角前往**DataWorks调度**，进行调度。

    ``` {#codeblock_j07_8f0_p1v .lanuage-sql}
    BEGIN;
    CREATE FOREIGN TABLE if not EXISTS bank_data_foreign_holo (
     age int8,
     job text,
     marital text,
     education text,
     card text,
     housing text,
     loan text,
     contact text,
     month text,
     day_of_week text,
     duration text,
     campaign int8,
     pdays float8,
     previous float8,
     poutcome text,
     emp_var_rate float8,
     cons_price_idx float8,
     cons_conf_idx float8,
     euribor3m float8,
     nr_employed float8,
     y int8
    )
    SERVER odps_server
    OPTIONS (project_name 'hologresdemo4rh_dev', table_name 'bank_data_odps');
    GRANT SELECT ON bank_data_foreign_holo TO PUBLIC;
    COMMIT;
    ```

3.  **外部表调度** 

    跳转到DataWorks调度页面之后，进行调度信息配置。

    **新建Hologres开发**之后，选中刚新建的开发节点，并单击**更新节点版本**，即可将HoloStudio中新建的节点同步至DataWorks，单击左侧**调度配置**，选中**调度依赖**，父节点输出名称确保为**MaxCompute源头表**。调度配置完成之后单击**保存** \> **提交** \> **发布**，发布成功后单击**运维中心**，前往生产环境发布。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/1644317/156820535759463_zh-CN.png)

    进入到生产环境后，，选中发布的节点，并单击**发布**。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/1644317/156820535859465_zh-CN.png)

    发布成功之后，单击右上角**运维中心**，进行数据配置。单击左侧菜单栏**周期任务**，选中发布的节点，右键单击**补数据** \> **当前节点**，即可成功发布。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/1644317/156820535859466_zh-CN.png)

4.  **HoloStudio建立分区表数据开发** 

    外部表节点发布完成之后，前往HoloStudio建立分区表数据开发，写入分区数据。

    进入**HoloStudio** \> **数据开发** \> **新建数据开发**，输入SQL命令，单击**运行**，并给自定义参数赋值，运行成功后，单击**保存** \> **前往DataWorks调度**，示例SQL如下。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/1644317/156820535859469_zh-CN.png)

    ``` {#codeblock_zh9_z4m_rz9 .lanuage-sql}
    BEGIN;
    CREATE TABLE if not EXISTS bank_data_holo (
     age int8,
     job text,
     marital text,
     education text,
     card text,
     housing text,
     loan text,
     contact text,
     month text,
     day_of_week text,
     duration text,
     campaign int8,
     pdays float8,
     previous float8,
     poutcome text,
     emp_var_rate float8,
     cons_price_idx float8,
     cons_conf_idx float8,
     euribor3m float8,
     nr_employed float8,
     y int8,
     ds text NOT NULL
    )
    PARTITION  BY LIST(ds);
    CALL SET_TABLE_PROPERTY('bank_data_holo', 'orientation', 'column');
    CALL SET_TABLE_PROPERTY('bank_data_holo', 'time_to_live_in_seconds', '700000');
    COMMIT;
    
    
    
    create table if not exists bank_data_holo_1_${bizdate} partition of bank_data_holo
      for values in ('${bizdate}');
    
    insert into bank_data_holo_1_${bizdate}
    select 
        age as age,
        job as job,
        marital as marital,
        education as education,
        card as card,
         housing as housing,
        loan as loan,
        contact as contact,
        month as month,
        day_of_week as day_of_week,
         duration as duration,
        campaign as campaign,
         pdays as pdays,
        previous as previous,
        poutcome as poutcome,
         emp_var_rate as emp_var_rate,
        cons_price_idx as cons_price_idx,
        cons_conf_idx as cons_conf_idx,
        euribor3m as euribor3m,
        nr_employed as nr_employed,
        y as y,
        '${bizdate}' as ds 
    from bank_data_foreign_holo;
    ```

5.  **分区表调度** 

    跳转至DataWorks**新建数据开发**，单击**更新节点版本**，将分区表信息同步同步至该节点，并单击右侧**调度配置**，将**基础属性** \> **参数赋值**为某个时间，**调度依赖**为刚发布的**外部表**，完成之后，单击**保存** \> **提交** \> **发布**，并前往**运维中心**进行生产环境发布。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/1644317/156820535859470_zh-CN.png)

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/1644317/156820535859471_zh-CN.png)

6.  **发布并周期性调度数据** 

    将分区表开发节点发布成功之后，再将数据补充完整。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/1644317/156820535859472_zh-CN.png)

    前往HoloStudio，单击左侧菜单栏**PG管理** \> **表**，选中调度配置成功的分区表，并单击**数据预览**，即可查看到数据。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/1644317/156820535959473_zh-CN.png)


