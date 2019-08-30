# 常见BI连接工具 {#concept_1664385 .concept}

本文将为您介绍交互式分析（Interactive Analytics）常见的BI连接工具。

## DataWorks数据服务 {#section_2ed_0gr_wyk .section}

DataWorks数据服务旨在为企业搭建统一的数据服务总线，帮助企业统一管理对内对外的API服务。数据服务为您提供快速将数据表生成数据API的能力，同时支持您将现有的API快速注册到数据服务平台以统一管理和发布。

数据服务目前支持2种数据源方式连接交互式分析（Interactive Analytics），一个是标准的“POSTGRESQL”数据源，一种是“Ligtning”数据源。配置如下。

1.  登录**DataWorks** \> **数据集成** \> **数据源**。
2.  单击数据源页面右上角的**新增数据源** \> **Lightning**数据源或者**PostgreSQL**数据源。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/1345947/156716895955950_zh-CN.png)


**说明：** ：

-   Endpoint：申请的环境域名。
-   端口号：申请的环境port。
-   dbname：访问的数据库名。
-   xxx：访问用户的Access ID。
-   yyy：访问用户的Access Key 。

## SQL Workbench/J {#section_n7q_pai_t4c .section}

SQL Workbench/J是一款流行的免费、跨平台SQL查询分析工具，使用SQL Workbench/J可以通过PostgreSQL驱动连接Interactive Analytics服务。

1.  下载并安装SQL Workbench/J。
2.  启动SQL Workbench/J，创建数据库连接。

选择PostgreSQL驱动，连接交互式分析（Interactive Analytics）项目所对应的Interactive Analytics URL地址，同时输入访问用户的用户名和密码，即Access ID和Access Key。

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/1345947/156716895955521_zh-CN.png)

也可通过扩展属性（Extended Properities）设置ssl取值为true。

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/1345947/156716895955522_zh-CN.png)

连接后，在Workbench工作区查看交互式分析（Interactive Analytics）项目的表数据、查询分析。

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/1345947/156716895955523_zh-CN.png)

## Tableau Desktop {#section_d6z_37p_gvm .section}

使用BI工具，选择PostgreSQL数据源，配置连接。配置连接时，需勾选需要SSL。

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/1345947/156716895955652_zh-CN.jpg)

登录后，通过Tableau创建工作表进行可视化分析。

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/1345947/156716895955666_zh-CN.jpg)

为了获得更好的性能和体验，建议您使用Tableau支持的TDC文件方式，对Interactive Analytics数据源进行连接定制优化。具体操作如下。

1.  将如下xml内容保存为postgresql.tdc文件。

    ``` {#codeblock_7i0_okg_k8a}
    <?xml version='1.0' encoding='utf-8' ?>
    <connection-customization class='postgres' enabled='true' version='8.10'>
    <vendor name='postgres'></vendor>
    <driver name='postgres'></driver>
    <customizations>
    <customization name='CAP_CREATE_TEMP_TABLES' value='no' ></customization><customization name='CAP_STORED_PROCEDURE_TEMP_TABLE_FROM_BUFFER' value='no' ></customization>
    <customization name='CAP_CONNECT_STORED_PROCEDURE' value='no' ></customization>
    <customization name='CAP_SELECT_TOP_INTO' value='no' ></customization>
    <customization name='CAP_ISOLATION_LEVEL_SERIALIZABLE' value='yes' ></customization>
    <customization name='CAP_SUPPRESS_DISCOVERY_QUERIES' value='yes' ></customization>
    <customization name='CAP_SKIP_CONNECT_VALIDATION' value='yes' ></customization>
    <customization name='CAP_ODBC_TRANSACTIONS_SUPPRESS_EXPLICIT_COMMIT' value='yes' ></customization>
    <customization name='CAP_ODBC_TRANSACTIONS_SUPPRESS_AUTO_COMMIT' value='yes' ></customization>
    <customization name='CAP_ODBC_REBIND_SKIP_UNBIND' value='yes' ></customization>
    <customization name='CAP_FAST_METADATA' value='no' ></customization>
    <customization name='CAP_ODBC_METADATA_SUPPRESS_SELECT_STAR' value='yes' ></customization>
    <customization name='CAP_ODBC_METADATA_SUPPRESS_EXECUTED_QUERY' value='yes' ></customization>
    <customization name='CAP_ODBC_UNBIND_AUTO' value='yes' ></customization>
    <customization name='SQL_TXN_CAPABLE' value='0' ></customization>
    <customization name='CAP_ODBC_CURSOR_FORWARD_ONLY' value='yes' ></customization>
    <customization name='CAP_ODBC_TRANSACTIONS_COMMIT_INVALIDATES_PREPARED_QUERY' value='yes' ></customization>
    </customizations>
    </connection-customization>
    						
    ```

2.  将文件保存到\\My Documents\\My Tableau Repository\\Datasources目录下。如果是Tableau Server，Windows下请保存在C:\\ProgramData\\Tableau\\Tableau Server\\data\\tabsvc\\vizqlserver\\Datasources，Linux下请保存在/var/opt/tableau/tableau\_server/data/tabsvc/vizqlserver/Datasources/。
3.  重新打开Tableau，使用PostgreSQL数据源连接Interactive Analytics服务。关于TDC文件定制数据源的更多内容，请参见[Tableau官方帮助文档](https://onlinehelp.tableau.com/current/pro/desktop/en-us/odbc_customize.html?spm=a2c4g.11186623.2.25.1bff63e4h9iIrY#global-tdc)。

