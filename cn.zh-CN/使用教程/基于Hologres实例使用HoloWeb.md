---
keyword: [Hologres, HoloWeb, 使用教程]
---

# 基于Hologres实例使用HoloWeb

HoloWeb用于处理交互式分析Hologres数据。使用HoloWeb时，您需要提前开通Hologres实例并且连接至HoloWeb。本文为您介绍如何开通Hologres实例、连接Hologres实例至HoloWeb并使用HoloWeb开发数据。

-   阿里云账号注册，详情请参见[t12832.md\#]()。
-   实名认证，详情请参见[t12833.md\#]()或[t12834.md\#]()。

## 开通Hologres

**说明：** 如果您已经开通Hologres，请跳过该步骤，直接连接Hologres实例至HoloWeb。

1.  登录[阿里云官网](https://www.aliyun.com/)，单击右上角的**登录**，输入您的阿里云账号和密码。

2.  鼠标悬停至顶部菜单栏中的**产品分类**，单击**大数据** \> **Hologres**，进入Hologres产品详情页。

3.  单击**立即购买**。

4.  进入**包年包月**页面，输入**基本配置**和**购买量**，单击**立即购买**。


## 使用Hologres管理控制台新建数据库

开通Hologres实例后，系统自动创建postgres数据库。

该数据库分配到的资源较少，仅用于管理。您需要新建数据库来处理实际业务。

1.  进入[Hologres管理控制台](https://hologram.console.aliyun.com/#/instance)，单击**实例列表** \> **Hologres引擎管理**界面已购买的实例名称，进入实例详情页。

2.  单击左侧菜单栏**DB管理** \> **新建Database**，输入**Database名称**，勾选开启**简单权限模型**，单击**确定**。


## 新建HoloWeb数据连接

1.  使用阿里云账号登录[HoloWeb](https://account.aliyun.com/login/login.htm?oauth_callback=http%3A%2F%2Fholoweb-cn-shanghai.data.aliyun.com%2F)。

2.  单击**连接管理** \> **数据连接**。

3.  配置**新建连接**弹框的参数，单击**确认**。 连接成功后，可以在**连接管理** \> **我的连接**中查看数据库信息。

    |参数名称|说明|
    |----|--|
    |连接名称|自定义的连接名称。|
    |连接描述|连接的描述信息。|
    |主机|Hologres实例的公共网络域名。|
    |端口|Hologres实例的公共网络端口。|
    |初始化数据库|Hologres实例中创建的数据库名称。|
    |用户名|当前账号的AccessKey ID。|
    |密码|当前账号的AccessKey Secret。|
    |测试连通性|检测数据连接是否成功。|

    **说明：**

    -   进入[Hologres管理控制台](https://hologram.console.aliyun.com/#/instance)的实例详情页，从**实例配置**获取主机和端口，从**DB配置**获取初始化数据库。
    -   单击[AccessKey 管理](https://usercenter.console.aliyun.com/?spm=5176.2020520153.nav-right.dak.3bcf415dCWGUBj#/manage/ak)，获取用户名和密码。

## 使用HoloWeb新建数据库

1.  使用阿里云账号登录[HoloWeb](https://account.aliyun.com/login/login.htm?oauth_callback=http%3A%2F%2Fholoweb-cn-shanghai.data.aliyun.com%2F)。

2.  单击**连接管理** \> **数据库**。

3.  配置**连接名称**、**数据库名称**和**权限管理**参数，单击**确认**。

    **说明：** 配置的数据库名称必须唯一。


## 新建模式

1.  使用阿里云账号登录[HoloWeb](https://account.aliyun.com/login/login.htm?oauth_callback=http%3A%2F%2Fholoweb-cn-shanghai.data.aliyun.com%2F)。

2.  单击**连接管理** \> **模式**。

3.  配置**连接名称**、**数据库名称**和**模式名称**参数，单击**确认**。


## 新建内部表

1.  使用阿里云账号登录[HoloWeb](https://account.aliyun.com/login/login.htm?oauth_callback=http%3A%2F%2Fholoweb-cn-shanghai.data.aliyun.com%2F)。

2.  单击**连接管理** \> **表**。

3.  配置**新建内部表**的各项参数，单击**提交**。

    ![canshu](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/2273749951/p113938.png)


## 新建外部表

1.  使用阿里云账号登录[HoloWeb](https://account.aliyun.com/login/login.htm?oauth_callback=http%3A%2F%2Fholoweb-cn-shanghai.data.aliyun.com%2F)。

2.  单击**连接管理** \> **外部表**。

3.  配置**新建外部表**的各项参数，单击**提交**。

    ![waib](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/3273749951/p113940.png)

    **说明：** HoloWeb目前仅支持使用MaxCompute类型的外部表。


## 新建文件夹

1.  使用阿里云账号登录[HoloWeb](https://account.aliyun.com/login/login.htm?oauth_callback=http%3A%2F%2Fholoweb-cn-shanghai.data.aliyun.com%2F)。

2.  单击**Query查询** \> **文件夹**。

3.  配置**文件夹名称**和**目录**参数，单击**确认**。


## 新建SQL查询

1.  使用阿里云账号登录[HoloWeb](https://account.aliyun.com/login/login.htm?oauth_callback=http%3A%2F%2Fholoweb-cn-shanghai.data.aliyun.com%2F)。

2.  单击**Query查询** \> **SQL窗口**。

3.  配置**新建SQL查询**弹框的参数，单击**确认**。

    新建SQL查询后，在对应查询文件的编辑界面输入需要查询的SQL语句。

    ![sq](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/3273749951/p113942.png)

    **说明：** 新建SQL查询前，需要先新建存放查询的文件夹。


