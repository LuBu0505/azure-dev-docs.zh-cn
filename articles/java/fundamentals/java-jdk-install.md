---
title: 安装适用于 Azure 和 Azure Stack 的 Azul Zulu JDK
description: 如何安装 Azul Zulu Java 开发工具包 (JDK)，以便在 Windows、Linux 和 Mac 中进行 Azure 开发
ms.date: 04/19/2019
ms.topic: conceptual
ms.custom: devx-track-java
ms.openlocfilehash: e5b9f0874202373400640687df2b1558f7f41526
ms.sourcegitcommit: 9e282fc2ec967bee181c3034e7e70b28ae308905
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/04/2020
ms.locfileid: "89473483"
---
# <a name="install-the-jdk-for-azure-and-azure-stack"></a>安装适用于 Azure 和 Azure Stack 的 JDK

Azul Zulu for Azure - Enterprise Edition 内部版 JDK 是适用于 Azure 和 Azure Stack 的 OpenJDK 的免费、多平台、生产就绪型发行版，由 Microsoft 及 Azul Systems 提供支持。 这些版本包含构建和运行 Java SE 应用程序所需的所有组件。

有[多个支持用于每个客户端 OS 的下载包类型](https://www.azul.com/downloads/azure-only/zulu/)。 也可[从 Azure 市场库获取虚拟机映像](#get-virtual-machine-images-from-the-azure-marketplace-gallery)。

> [!NOTE]
> 以下说明针对 64 位 Java 8 版 JDK。 Azul 还提供 Java Run-time Environment (JRE) 作为独立安装。 此 JRE 随附在 JDK 安装中。
>
> Java 11 包也在 [Azul 的 Azure 下载页](https://www.azul.com/downloads/azure-only/zulu/)上提供。

## <a name="download-and-install-the-azul-zulu-for-azure---enterprise-edition-jdk-builds-for-windows"></a>下载并安装用于 Windows 的 Azul Zulu for Azure - Enterprise Edition JDK 内部版

1. 下载 MSI 文件格式的 64 位 Azul Zulu JDK 8：

   * [zulu-8-azure-jdk_8.44.0.11-8.0.242-win_x64.msi](http://repos.azul.com/azure-only/zulu/packages/zulu-8/8u242/zulu-8-azure-jdk_8.44.0.11-8.0.242-win_x64.msi)
   * 或者查找[更高 Zulu 8 版本](http://repos.azul.com/azure-only/zulu/packages/zulu-8)的 .msi  文件。

   将其保存到客户端上的某个位置，例如 `C:\Users\<your_login>\Downloads`。 （.ZIP 包也在 [Azul 的 Azure 下载页](https://www.azul.com/downloads/azure-only/zulu/)上提供。）

2. 导航到该目录，双击下载的 MSI 文件即可开始安装。

## <a name="download-and-install-the-azul-zulu-for-azure---enterprise-edition-jdk-builds-for-mac"></a>下载并安装用于 Mac 的 Azul Zulu for Azure - Enterprise Edition JDK 内部版

这些步骤将 ZIP 文件下载到 Mac。 此外还提供 DMG 版本。

1. 下载 ZIP 文件格式的 64 位 Azul Zulu JDK 8：

   * [zulu-8-azure-jdk_8.44.0.11-8.0.242-macosx_x64.zip](http://repos.azul.com/azure-only/zulu/packages/zulu-8/8u242/zulu-8-azure-jdk_8.44.0.11-8.0.242-macosx_x64.zip)
   * 或者查找[更高 Zulu 8 版本](http://repos.azul.com/azure-only/zulu/packages/zulu-8)的 .zip  文件。

   将其保存到客户端上的某个位置，例如 `/Library/Java/JavaVirtualMachines/`。 （.DMG 包也在 [Azul 的 Azure 下载页](https://www.azul.com/downloads/azure-only/zulu/)上提供。）

2. 启动 Finder，导航到下载目录，然后双击 ZIP 文件。 也可启动终端命令窗口，然后导航到目录并运行：

    ```cli
    unzip <name_of_zulu_package>.zip
    ```

## <a name="download-and-install-the-azul-zulu-for-azure---enterprise-edition-jdk-builds-for-alpine-linux"></a>下载并安装用于 Alpine Linux 的 Azul Zulu for Azure - Enterprise Edition JDK 内部版

1. 下载 TAR 文件格式的 64 位 Azul Zulu JDK 8

   * [zulu-8-azure-jdk_8.44.0.11-8.0.242-linux_x64.tar.gz](http://repos.azul.com/azure-only/zulu/packages/zulu-8/8u242/zulu-8-azure-jdk_8.44.0.11-8.0.242-linux_x64.tar.gz)
   * 或者查找[更高 Zulu 8 版本](https://repos.azul.com/azure-only/zulu/packages/zulu-8)的 .tar.gz  文件。

   将其保存到客户端上的某个位置，例如 `/usr/lib/jvm`。 （.RPM 和 .DEB 包也在 [Azul 的 Azure 下载页](https://www.azul.com/downloads/azure-only/zulu/)上提供。）

2. 转到你的目录并运行以下命令，将文件解压缩并展开：

    ```cli
    tar -xvf <name_of_zulu_package>.tar
    ```

## <a name="confirm-your-installation"></a>确认安装

若要确认安装，请转到命令行并运行 `java -version`。

命令的输出应如以下示例所示：

```cli
$ java -version

openjdk version "1.8.0_242"
OpenJDK Runtime Environment (Zulu 8.44.0.11-linux64)-Microsoft-Azure-restricted (build 1.8.0_242-b20)
OpenJDK 64-Bit Server VM (Zulu 8.44.0.11-linux64)-Microsoft-Azure-restricted (build 25.242-b20, mixed mode)
```

## <a name="get-virtual-machine-images-from-the-azure-marketplace-gallery"></a>从 Azure 市场库获取虚拟机映像

可以获取以下平台的虚拟机映像：

* [Azul Zulu：基于 Ubuntu 20.04 的 Java 8](https://azuremarketplace.microsoft.com/marketplace/apps/azul.azul-zulu11-ubuntu-2004?tab=Overview)
* [Azul Zulu：基于 Windows Server 2019 的 Java 8](https://azuremarketplace.microsoft.com/marketplace/apps/azul.azul-zulu8-windows-2019)
* [Azul Zulu：基于 Ubuntu 20.04 的 Java 11](https://azuremarketplace.microsoft.com/marketplace/apps/azul.azul-zulu11-ubuntu-2004?tab=Overview)
* [Azul Zulu：基于 Windows Server 2019 的 Java 11](https://azuremarketplace.microsoft.com/marketplace/apps/azul.azul-zulu11-windows-2019)

## <a name="download-and-install-the-azul-zulu-for-azure---enterprise-edition-jdks-from-a-yum-repository"></a>下载并安装 Yum 存储库中的 Azul Zulu for Azure - Enterprise Edition JDK

Azul Zulu JDK 在 [Yum 存储库](https://repos.azul.com/azure-only/zulu-azure.repo)中由 Azul 提供。

**若要安装适用于 Java 8 的 Azul Zulu JDK，请在 CLI 中运行以下命令：**

```cli
sudo rpm --import http://repos.azul.com/azul-repo.key
sudo curl http://repos.azul.com/azure-only/zulu-azure.repo -o /etc/yum.repos.d/zulu-azure.repo
sudo yum -q -y update
sudo yum -q -y install zulu-8-azure-jdk
```

对于 Java 11，请运行：

```cli
sudo rpm --import http://repos.azul.com/azul-repo.key
sudo curl http://repos.azul.com/azure-only/zulu-azure.repo -o /etc/yum.repos.d/zulu-azure.repo
sudo yum -q -y update
sudo yum -q -y install zulu-11-azure-jdk
```

对于 Java 13（预览版），请运行：

```cli
sudo rpm --import http://repos.azul.com/azul-repo.key
sudo curl http://repos.azul.com/azure-only/zulu-azure.repo -o /etc/yum.repos.d/zulu-azure.repo
sudo yum -q -y update
sudo yum -q -y install zulu-13-azure-jdk
```

**若要更新 Yum 存储库中的 Zulu JDK 8 包，请运行以下命令：**

```cli
sudo yum -q -y install zulu-8-azure-jdk
```

（如果使用其他版本，请更改以上命令中的版本号。）

**若要删除 Yum 存储库中的 Zulu JDK 8 包，请运行以下命令：**

```cli
sudo yum -y erase zulu-8-azure-jdk
```

（如果使用其他版本，请更改以上命令中的版本号。）

## <a name="download-and-install-the-azul-zulu-jdks-from-an-apt-get-repository"></a>下载并安装 apt-get 存储库中的 Azul Zulu JDK

Azul Zulu JDK 也在 [apt-get 存储库](https://repos.azul.com/azure-only/zulu/apt)中由 Azul 提供。

**若要通过 apt-get 安装适用于 Java 8 的 Azul Zulu JDK，请在 CLI 中运行以下命令：**

```cli
sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv-keys 0xB1998361219BD9C9
sudo apt-add-repository "deb http://repos.azul.com/azure-only/zulu/apt stable main"
sudo apt-get -q update
sudo apt-get -y install zulu-8-azure-jdk
```

对于 Java 11，请运行：

```cli
sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv-keys 0xB1998361219BD9C9
sudo apt-add-repository "deb http://repos.azul.com/azure-only/zulu/apt stable main"
sudo apt-get -q update
sudo apt-get -y install zulu-11-azure-jdk
```

对于 Java 13（预览版），请运行：

```cli
sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv-keys 0xB1998361219BD9C9
sudo apt-add-repository "deb http://repos.azul.com/azure-only/zulu/apt stable main"
sudo apt-get -q update
sudo apt-get -y install zulu-13-azure-jdk
```

**若要更新 apt-get 存储库中的 Zulu JDK 8 包，请运行以下命令：**

```cli
sudo apt-get -q update
sudo apt-get -y install zulu-8-azure-jdk
```

以前的版本会被自动删除。
（如果使用其他版本，请更改以上命令中的版本号。）

**若要删除 apt-get 存储库中的 Zulu JDK 8 包，请运行以下命令：**

```cli
sudo apt-get -y purge zulu-8-azure-jdk
```

（如果使用其他版本，请更改以上命令中的版本号。）

若要更详细地了解如何准备、安装和管理用于 Azure 开发的 Azul Zulu JDK，请阅读[官方 Zulu 文档](https://docs.azul.com/zulu/zuludocs/index.htm)。
