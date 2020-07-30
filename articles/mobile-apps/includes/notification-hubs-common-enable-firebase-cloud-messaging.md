---
author: mikeparker104
ms.author: miparker
ms.date: 07/27/2020
ms.service: mobile-services
ms.topic: include
ms.openlocfilehash: fb2311fe9256daa53b2687c43b508c4af4f97bd2
ms.sourcegitcommit: cf23d382eee2431a3958b1c87c897b270587bde0
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/29/2020
ms.locfileid: "87401404"
---
### <a name="create-a-firebase-project-and-enable-firebase-cloud-messaging-for-android"></a>创建 Firebase 项目并为 Android 启用 Firebase Cloud Messaging

1. 登录到 [Firebase 控制台](https://firebase.google.com/console/)。 创建一个新的 Firebase 项目，输入 PushDemo 作为项目名称 。

    > [!NOTE]
    > 将生成唯一名称。 默认情况下，此名称由提供的名称小写变体和用短划线分隔的生成数字组成。 如果需要可以对其进行更改，但前提是它仍是全局唯一名称。

1. 创建项目后，选择“向 Android 应用添加 Firebase”。

    ![将 Firebase 添加到 Android 应用](../media/notification-hubs-add-firebase-to-android-app.png)

1. 在“将 Firebase 添加到 Android 应用”页上，执行以下步骤。
    1. 对于“Android 包名称”，请输入包的名称。 例如：`com.<organization_identifier>.<package_name>`。

        ![指定程序包名称](../media/specify-package-name-firebase-cloud-messaging-settings.png)
    1. 选择“注册应用”。  
    1. 选择“下载 google-services.json”。 然后将该文件保存到本地文件夹中供稍后使用，并选择“下一步”。  

        ![下载 google-services.json](../media/download-google-service-button.png)
    1. 选择“**下一页**”。
    1. 选择“继续访问控制台”

        > [!NOTE]
        > 如果由于“验证安装”检查未启用“继续访问控制台”按钮，请选择“跳过此步骤”。

1. 在 Firebase 控制台中，选择与项目相对应的齿轮图标。 然后，选择“项目设置”。

    ![选择“项目设置”](../media/notification-hubs-firebase-console-project-settings.png)

    > [!NOTE]
    > 如果尚未下载 google-services.json 文件，可以在此页上进行下载。

1. 切换到顶部的“Cloud Messaging”选项卡。 复制并保存**服务器密钥**以供将来使用。 你将使用此值来配置通知中心。

    ![复制服务器密钥](../media/server-key.png)
