---
keyword: [create an Alibaba Cloud account, AccessKey pair]
---

# Create an Alibaba Cloud account

This topic describes how to create an Alibaba Cloud account and create an AccessKey pair for the account.

1.  Create an Alibaba Cloud account

    Go to the [international site \(alibabacloud.com\)](https://www.alibabacloud.com/) and click **Free Account** in the upper-right corner. On the page that appears, enter the required information to create an Alibaba Cloud account.

    **Note:**

    -   The Alibaba Cloud account owner has full operational control over all purchased resources. To keep your account safe, we recommend that you regularly update your password and do not share your account with others.
    -   You can purchase and connect to a Hologres instance only by using your Alibaba Cloud account.
2.  Complete real-name verification.

    Go to the Real-name Registration page in the [Account Management console](https://account-intl.console.aliyun.com/#/secure) and complete the verification.

    **Note:** You must complete real-name verification before you purchase and use Alibaba Cloud services.

3.  Create an AccessKey pair.

    To analyze data by using Hologres, you must create an AccessKey pair for your Alibaba Cloud account. Unlike the username and password that you use to log on to the Alibaba Cloud Management Console, an AccessKey pair is used to connect Hologres to development or BI tools. An AccessKey pair consists of an AccessKey ID and an AccessKey secret. To create an AccessKey pair, perform the following steps:

    **Note:** After an AccessKey pair is created for your Alibaba Cloud account, keep the AccessKey ID and AccessKey secret safe. If the AccessKey ID or AccessKey secret is or may be disclosed, change the AccessKey pair in a timely manner.

    1.  Log on to the [Hologres console](https://hologram.console.aliyun.com/#/overview). Move the pointer over your profile picture in the upper-right corner and click **AccessKey Management**.

        ![AK](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/6730580061/p111042.png)

    2.  In the **Note** message, click **Use Current AccessKey Pair**.

        ![Note](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/6730580061/p111037.png)

    3.  On the **AccessKey Management** page, click **Create AccessKey**.

        ![Create AccessKey](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/7730580061/p111045.png)

    4.  In the **Phone Verification** dialog box, click **Send**.

        ![Phone Verification](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/7730580061/p111059.png)

    5.  Enter the verification code and click **OK**.

        ![Verification Code](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/7730580061/p111062.png)

    6.  In the **Create AccessKey** message, view the AccessKey pair that you created.


After you close the Create AccessKey message, view the status of the AccessKey pair on the **AccessKey Management** page. You can also click **Disable** or **Delete** in the Actions column to disable or delete the AccessKey pair.

![View the status of the AccessKey pair](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/7730580061/p111072.png)

**Note:** If you disable the AccessKey pair, all the services that use the AccessKey pair fail to be run and report errors. Therefore, after you change your AccessKey pair, you must check the running status of services that use the AccessKey pair and recover the services in a timely manner.

