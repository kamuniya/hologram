---
keyword: [Hologres, 新用户, 使用教程]
---

# 新用户基于DataWorks使用HoloStudio

HoloStudio用于处理交互式分析Hologres数据。新用户使用HoloStudio时，需要提前开通Hologres和DataWorks实例，并且绑定Hologres实例至DataWorks工作空间。本文为您介绍新用户如何开通实例、创建工作空间和开发数据。

-   阿里云账号注册，详情请参见[t12832.md\#]()。
-   实名认证，详情请参见[t12833.md\#]()或[t12834.md\#]()。

本次实验涉及的阿里云产品如下：

-   数据工场[DataWorks](https://data.aliyun.com/product/ide)。
-   交互式分析[Hologres](https://www.aliyun.com/product/hologram?spm=5176.10695662.1384920.1.db20204c2z1xfa)。

## 开通Hologres

**说明：** 如果您已经开通Hologres，请跳过该步骤，直接创建DataWorks工作空间。

1.  登录[阿里云官网](https://www.aliyun.com/)，单击右上角的**登录**，输入您的阿里云账号和密码。

2.  鼠标悬停至顶部菜单栏中的**产品分类**，单击**大数据** \> **Hologres**，进入Hologres产品详情页。

3.  单击**立即购买**。

4.  进入**包年包月**页面，输入**基本配置**和**购买量**，单击**立即购买**。


## 新建数据库

开通Hologres实例后，系统自动创建postgres数据库。该数据库分配到的资源较少，仅用于管理。您需要新建数据库来处理实际业务。

1.  进入[Hologres管理控制台](https://hologram.console.aliyun.com/#/instance)，单击**实例列表** \> **Hologres引擎管理**界面已购买的实例名称，进入实例详情页。

2.  单击左侧菜单栏**DB管理** \> **新建Database**，输入**Database名称**，勾选开启**简单权限模型**，单击**确定**。


## 开通DataWorks

1.  登录[阿里云官网](https://www.aliyun.com/)，单击右上角的**登录**，输入您的阿里云账号和密码。

2.  鼠标悬停至顶部菜单栏中的**产品分类**，单击**大数据** \> **DataWorks**，进入DataWorks产品详情页。

3.  单击**立即开通**。

4.  进入**DataWorks购买**页面，选择**地域**、**版本**和**购买时长**。勾选**服务协议**，单击**确认订单并支付**。

    **说明：** 购买DataWorks时，选择的地域要与Hologres的地域保持一致。


## 创建DataWorks工作空间

**说明：**

-   您可以选择将Hologres实例绑定至已有的DataWorks工作空间。
-   Hologres实例和DataWorks工作空间的地域必须相同。

1.  使用主账号登录[DataWorks控制台](https://workbench.data.aliyun.com/console)。

2.  在**概览**页面，单击右侧的**快速入口** \> **创建工作空间**。

    您也可以单击左侧导航栏中的工作空间列表，切换至相应的区域后，单击**创建工作空间**。

3.  配置创建工作空间对话框中的**基本配置**，单击**下一步**。

4.  进入**选择引擎**界面，勾选Hologres引擎后，单击**下一步**。

    DataWorks已正式商用，如果该区域没有开通，需要首先开通正式商用的服务。默认选中**数据集成**、**数据开发**、**运维中心**和**数据质量**。

5.  进入**引擎详情**页面，配置选购引擎的参数。

    |参数名称|说明|
    |----|--|
    |实例显示名称|自定义实例名称。|
    |访问身份|选择阿里云主账号或已授权的子账号。|
    |Hologres实例名称|选择已购买的实例。 **说明：** 请确保Hologres实例与DataWorks工作空间的地域相同。 |
    |数据库名称|输入已创建的数据库名称。|

6.  配置完成后，单击**创建工作空间**。

    工作空间创建成功后，即可在工作空间列表页面查看相应内容。


## 数据开发

1.  使用主账号登录[DataWorks控制台](https://workbench.data.aliyun.com/console)。

2.  单击**工作空间列表**界面已创建的工作空间名称，进入**DataStudio**。

3.  单击![data](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/4413119951/p113897.png)，选择**全部产品** \> **HoloStudio**，进入**HoloStudio**。

4.  单击**PG管理** \> **刷新**，显示成功绑定的数据库。


环境配置完成。您可以在HoloStudio中开发数据，详情请参见[概述](/cn.zh-CN/基于HoloStudio的开发/数据开发/概述.md)。

