# JDBC驱动程序 {#concept_1715039 .concept}

交互式分析（Interactive Analytics）提供完全兼容PostgreSQL消息协议的Java数据库连接（JDBC）接口，您可以通过JDBC将SQL客户端工具连接到Interactive Analytics服务。

**使用PostgreSQL官方提供的JDBC驱动程序**

很多客户端工具默认集成了PostgreSQL数据库的驱动，直接使用工具自带的驱动即可。如果未集成，可从[JDBC官网](https://jdbc.postgresql.org/?spm=a2c4g.11186623.2.7.3eaf12a8I631Qr)下载。以SQL Workbench/J客户端为例，创建连接时可选择PostgreSQL官方驱动。

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/1332511/156820530055608_zh-CN.png)

