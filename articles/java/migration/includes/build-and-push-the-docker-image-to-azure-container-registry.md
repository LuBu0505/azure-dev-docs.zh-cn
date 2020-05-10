---
author: mriem
ms.author: manriem
ms.date: 2/28/2020
ms.openlocfilehash: 1c7059df89281156861a935d451aca4db6f44418
ms.sourcegitcommit: be67ceba91727da014879d16bbbbc19756ee22e2
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/05/2020
ms.locfileid: "82109736"
---
### <a name="build-and-push-the-docker-image-to-azure-container-registry"></a>构建 Docker 映像并将其推送到 Azure 容器注册表

创建 Dockerfile 后，需要生成 Docker 映像并将其发布到 Azure 容器注册表。

如果你使用了我们的 [WildFly 容器快速入门 GitHub 存储库](https://github.com/Azure/wildfly-container-quickstart)，则构建映像并将其推送到 Azure 容器注册表的过程相当于调用以下三个命令。

在这些示例中，`MY_ACR` 环境变量保存 Azure 容器注册表的名称，`MY_APP_NAME` 变量保存要在 Azure 容器注册表中使用的 Web 应用程序的名称。

生成 WAR 文件：

```shell
mvn package
```

登录到 Azure 容器注册表：

```shell
az acr login -n ${MY_ACR}
```

生成并推送映像：

```shell
az acr build -t ${MY_ACR}.azurecr.io/${MY_APP_NAME} -f src/main/docker/Dockerfile .
```

也可使用 Docker CLI 在本地生成并测试映像，如以下命令所示。 此方法可以简化在一开始部署到 ACR 之前对映像进行的测试和优化。 但是，它要求安装 Docker CLI 并确保 Docker 后台程序正在运行。

生成映像：

```shell
docker build -t ${MY_ACR}.azurecr.io/${MY_APP_NAME}
```

在本地运行映像：

```shell
docker run -it -p 8080:8080 ${MY_ACR}.azurecr.io/${MY_APP_NAME}
```

现在可以在 `http://localhost:8080` 上访问你的应用程序。

登录到 Azure 容器注册表：

```shell
az acr login -n ${MY_ACR}
```

向 Azure 容器注册表推送映像：

```shell
docker push ${MY_ACR}.azurecr.io/${MY_APP_NAME}
```

若要更深入地了解如何在 Azure 中生成和存储容器映像，请参阅 Learn 模块：[使用 Azure 容器注册表生成和存储容器映像](/learn/modules/build-and-store-container-images/)。
