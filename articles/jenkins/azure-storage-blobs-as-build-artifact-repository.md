---
title: 教程 - 将 Azure 存储用于生成项目
description: 了解如何使用 Azure Blob 服务作为 Jenkins 持续集成解决方案创建的生成项目的存储库。
keywords: jenkins, azure, devops, 存储, cicd, 生成项目
ms.topic: article
ms.date: 11/19/2020
ms.custom: devx-track-jenkins
ms.openlocfilehash: 22ce5c0ee00af64bd5bf8dc43f0df1ad1b5e2400
ms.sourcegitcommit: 4dac39849ba2e48034ecc91ef578d11aab796e58
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/20/2020
ms.locfileid: "96035395"
---
# <a name="tutorial-use-azure-storage-for-build-artifacts"></a>教程：将 Azure 存储用于生成项目

[!INCLUDE [jenkins-integration-with-azure.md](includes/jenkins-integration-with-azure.md)]

本文介绍了如何将 Blob 存储用作 Jenkins 持续集成 (CI) 解决方案创建的生成项目的存储库，或者用作要在生成过程中使用的可下载文件的源。 在以下情况下，会发现此解决方案很有用：你在敏捷开发环境进行编码（使用 Java 或其他语言），生成基于持续集成运行并且需要一个适用于生成项目的存储库，以便（举例来说）你能与其他组织成员、客户共享生成项目或维护存档。 另一种情况是，当生成作业本身需要其他文件时，例如需要下载依赖项作为生成输入的一部分时。

在本教程中，将使用 Microsoft 提供的适用于 Jenkins CI 的 Azure 存储插件。

## <a name="jenkins-overview"></a>Jenkins 概述

Jenkins 通过允许开发人员集成其代码更改以及自动生成版本，实现了软件项目的持续集成。 此模式减少了冲突，提高了团队工作效率。 版本是版本控制的，可将生成工件上传到各种存储库中。 本文将演示如何将 Azure Blob 存储用作生成工件的存储库。 你还将了解如何从 Azure Blob 存储下载依赖项。

有关 Jenkins 的更多信息，请访问 [Jenkins 概览](https://wiki.jenkins-ci.org/display/JENKINS/Meet+Jenkins)。

## <a name="benefits-of-using-the-blob-service"></a>使用 Blob 服务的好处

使用 Blob 服务承载敏捷开发生成项目的好处包括：

* 生成工件或可下载依赖项的高可用性。
* Jenkins CI 解决方案上传生成项目时的性能。
* 客户和合作伙伴下载生成项目时的性能。
* 通过选择匿名访问、基于过期的共享访问、签名访问、专用访问等来控制用户访问策略。

## <a name="prerequisites"></a>先决条件

- **Azure 订阅**：如果还没有 Azure 订阅，可以在开始前[创建一个免费 Azure 帐户](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio)。
- **Jenkins 服务器**：如果未安装 Jenkins 服务器，请[在 Azure 上创建 Jenkins 服务器](./configure-on-linux-vm.md)。
- **Azure 存储帐户**：如果还没有存储帐户，请[创建存储帐户](/azure/storage/common/storage-account-create)。

## <a name="configure-your-environment"></a>配置环境

1. 如果当前没有 Jenkins CI 解决方案，可以使用以下技术运行一个 Jenkins CI 解决方案：
  
  1. 在已启用 Java 的计算机上，从 <https://jenkins-ci.org> 下载 jenkins.war。
  2. 在打开到包含 jenkins.war 的文件夹的命令提示符处，运行：
     
      `java -jar jenkins.war`

  3. 浏览到 `http://localhost:8080/` 以打开 Jenkins 仪表板。 使用 Jenkins 仪表板安装并配置 Azure 存储插件。
     
## <a name="how-to-use-the-blob-service-with-jenkins-ci"></a>如何将 Blob 服务用于 Jenkins CI

要将 Blob 服务用于 Jenkins，需要安装 Azure 存储插件，并对该插件进行配置以使用你的存储帐户，并创建一个将生成项目上载到存储帐户的生成后操作。 将在下面各节中介绍这些步骤。

## <a name="how-to-install-the-azure-storage-plugin"></a>如何安装 Azure 存储插件

1. 在 Jenkins 仪表板中，选择“管理 Jenkins”。
2. 在“管理 Jenkins”页面上，选择“管理插件”。
3. 选择“可用”选项卡。
4. 在“项目上传程序”部分中，选择“Microsoft Azure 存储插件”。
5. 选择“安装而不重启”或“立即下载并在重启后安装”。
6. 重新启动 Jenkins。

## <a name="how-to-configure-the-azure-storage-plugin-to-use-your-storage-account"></a>如何配置 Azure 存储插件以使用存储帐户

1. 在 Jenkins 仪表板中，选择“管理 Jenkins”。
2. 在“管理 Jenkins”页中，选择“配置系统”。
3. 在“Microsoft Azure 存储帐户配置”部分中：
   1. 输入存储帐户名，该帐户名可从 [Azure 门户](https://portal.azure.com)获取。
   2. 输入存储帐户密钥，该密钥同样可从 [Azure 门户](https://portal.azure.com)获取。
   3. 如果要使用全局 Azure 云，请对“BLOB 服务终结点 URL”使用默认值。 如果要使用其他 Azure 云，请使用在 [Azure 门户](https://portal.azure.com)中为存储帐户指定的终结点。 
   4. 选择“验证存储凭据”以验证存储帐户。 
   5. [可选] 如果有其他存储帐户并且希望其可供 Jenkins CI 使用，请选择“添加更多存储帐户”。
   6. 选择“保存”以保存设置。

## <a name="how-to-create-a-post-build-action-that-uploads-your-build-artifacts-to-your-storage-account"></a>如何创建将生成项目上传到存储帐户的后期生成操作

为了进行说明，首先需要创建一个将创建若干文件的作业，然后添加后期生成操作以将文件上传到存储帐户。

1. 在 Jenkins 仪表板中，选择“新建项”。
2. 将此作业命名为“MyJob”，选择“生成自由格式的软件项目”，然后选择“确定”。
3. 在作业配置的“生成”部分，选择“添加生成步骤”并选择“执行 Windows 批处理命令”。
4. 在“命令”中，使用下列命令：

    ```   
    md text
    cd text
    echo Hello Azure Storage from Jenkins > hello.txt
    date /t > date.txt
    time /t >> date.txt
    ```

5. 在作业配置的“生成后操作”部分中，选择“添加生成后操作”并选择“将项目上传到 Azure Blob 存储”。
6. 对于“存储帐户名称”，请选择要使用的存储帐户。
7. 对于“容器名称”，请指定容器名称。 （如果上传生成工件时，容器不存在，则将创建容器。）可使用环境变量，因此在此示例中，请输入 `${JOB_NAME}` 作为容器名称。
   
    > [!TIP]
    > 在为“执行 Windows 批处理命令”输入脚本的“命令”部分下方，有一个指向 Jenkins 识别的环境变量的链接。 选择此链接可了解环境变量名称和说明。 不允许将包含特殊字符的环境变量（如 BUILD_URL 环境变量）用作容器名称或通用虚拟路径。
    
8. 对于此示例，请选择“默认将新容器设为公开”。 （如果要使用私有容器，需要创建共享访问签名以允许访问，该内容超出本文讨论范围。 可在[使用共享访问签名 (SAS)](/azure/storage/common/storage-sas-overview)中了解有关共享访问签名的详细信息。）
9. [可选] 如果要在上传生成工件之前清除容器的内容，请选择“在上传前清除容器”（如果不希望清除容器的内容，则使该复选框保持未选中状态）。
10. 对于“要上传的项目列表”，请输入 `text/*.txt`。
11. 在本教程中，对于“已上传的项目的通用虚拟路径”，请输入 `${BUILD\_ID}/${BUILD\_NUMBER}`。
12. 选择“保存”以保存设置。
13. 在 Jenkins 仪表板中，选择“立即生成”以运行 MyJob。 检查控制台输出中的状态。 当生成后操作开始上传生成项目时，Azure 存储的状态消息将包括在控制台输出中。
14. 成功完成此作业后，可通过打开公共 Blob 检查生成项目。
    1. 登录 [Azure 门户](https://portal.azure.com)。
    2. 选择“存储”。
    3. 选择用于 Jenkins 的存储帐户名称。
    4. 选择“容器”。
    5. 选择名为 myjob 的容器，该名称是创建 Jenkins 作业时分配的作业名称的小写形式。 在 Azure 存储中，容器名称和 Blob 名称都是小写的（并且区分大小写）。 在名为 myjob 的容器的 Blob 列表中，应能看到 hello.txt 和 date.txt  。 复制这两项中任一项的 URL 并在浏览器中打开。 你会看到已作为生成工件上传的文本文件。

每个作业只能创建一个用来将项目上传到 Azure Blob 存储的生成后操作。 单个生成后操作用于将项目上传到 Azure Blob 存储，它可在“要上传的项目列表”中使用分号作为分隔符指定不同的文件（包括通配符）和文件路径。 例如，如果 Jenkins 内部版本在工作空间的 build 文件夹中生成了 JAR 文件和 TXT 文件，并且要将这两者都上传到 Azure Blob 存储，请对“要上传的项目列表”选项使用以下值：`build/\*.jar;build/\*.txt`。 还可以使用双冒号语法指定要在 Blob 名称内使用的路径。 例如，如果要在 Blob 路径中使用 binaries 上传 JAR 并在 Blob 路径中使用 notices 上传 TXT 文件，请对“要上传的项目列表”选项使用以下值：`build/\*.jar::binaries;build/\*.txt::notices`。

## <a name="how-to-create-a-build-step-that-downloads-from-azure-blob-storage"></a>如何创建从 Azure Blob 存储进行下载的生成步骤

以下步骤说明如何配置从 Azure Blob 存储下载项的生成步骤，这对在生成中添加项非常有用。 使用此模式的示例是 JAR，可能需要在 Azure blob 存储中保留 JAR。

1. 在作业配置的“生成”部分中，选择“添加生成步骤”并选择“从 Azure Blob 存储下载”。
2. 对于“存储帐户名称”，请选择要使用的存储帐户。
3. 对于“容器名称”，指定包含要下载的 Blob 容器的名称。 可以使用环境变量。
4. 对于“Blob 名称”，请指定 Blob 名称。 可以使用环境变量。 另外，在指定 Blob 名称的初始字母后，可以使用星号作为通配符。 例如，project\\_ 将指定名称以 project 开头的所有 blob。
5. [可选] 对于“下载路径”，指定希望将文件从 Azure Blob 存储下载到的 Jenkins 计算机路径。 也可以使用环境变量。 （如未提供“下载路径”的值，则会将 Azure Blob 存储中的文件下载到作业工作区。）

如果还希望从 Azure Blob 存储下载其他项，可以创建其他生成步骤。

在运行生成后，可以检查生成历史记录控制台输出或下载位置，看是否成功下载了需要的 Blob。  

## <a name="components-used-by-the-blob-service"></a>Blob 服务使用的组件

本部分概述了 Blob 服务组件。

* **存储帐户**：对 Azure 存储进行的所有访问都要通过存储帐户完成。 存储帐户是访问 blob 的最高级别的命名空间。 一个帐户可以包含无限个容器，只要这些容器的总大小不超过 100 TB 即可。
* **容器**：一个容器包含一组 blob 集。 所有 Blob 必须都在一个容器中。 一个帐户可以包含无限数量的容器。 一个容器可以存储无限数量的 Blob。
* **Blob**：任意类型和大小的文件。 可将两类 Blob 存储到 Azure 存储中：块 Blob 和页 Blob。 大部分文件都是块 blob。 单个块 Blob 最大可以为 200 GB。 本教程使用的是块 Blob。 另一种 blob 类型为页 blob，其大小可达 1 TB，对文件中的一系列字节进行频繁修改时，这种 blob 更加高效。 有关 Blob 的详细信息，请参阅 [Understanding Block Blobs, Append Blobs, and Page Blobs](/rest/api/storageservices/Understanding-Block-Blobs--Append-Blobs--and-Page-Blobs)（了解块 Blob、追加 Blob 和页 Blob）。
* **URL 格式**：可使用以下 URL 格式对 Blob 寻址：
  
    `http://storageaccount.blob.core.windows.net/container_name/blob_name`
  
**注释**：

- 以上格式适用于全局 Azure 云。 如果要使用其他 Azure 云，请使用 [Azure 门户](https://portal.azure.com)中的终结点来确定你的 URL 终结点。
- 占位符 `storageaccount` 表示存储帐户的名称。
- 占位符 `container_name` 表示容器的名称。
- 占位符 `blob_name` 表示 blob 的名称。
- 在容器名称中，可以包含多个路径。 这些路径由正斜杠分隔。 
- 本教程中使用的示例容器的名称是 MyJob。
- 通用虚拟路径定义为 ${BUILD\_ID}/${BUILD\_NUMBER}。 此值会导致 blob 具有以下格式的 URL：
  
    `http://example.blob.core.windows.net/myjob/2014-04-14_23-57-00/1/hello.txt`

## <a name="troubleshooting-the-jenkins-plugin"></a>排查 Jenkins 插件问题

如果 Jenkins 插件出现任何 bug，请在 [Jenkins JIRA](https://issues.jenkins-ci.org/) 中提出特定组件的问题。

## <a name="next-steps"></a>后续步骤

> [!div class="nextstepaction"]
> [Azure 上的 Jenkins](/azure/Jenkins/)