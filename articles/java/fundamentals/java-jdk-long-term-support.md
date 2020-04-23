---
title: 针对 Azure 开发的 Java JDK 和长期支持
description: 开发和运行 Java 应用程序所需的 Azure 支持的下载内容和声明。
ms.date: 04/09/2019
ms.topic: conceptual
ms.custom: seo-java-september2019
ms.openlocfilehash: 02915383dd72a18959dbb5ba0f925a0e10b691d7
ms.sourcegitcommit: 0af39ee9ff27c37ceeeb28ea9d51e32995989591
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/21/2020
ms.locfileid: "81670643"
---
# <a name="java-long-term-support-for-azure-and-azure-stack"></a>针对 Azure 和 Azure Stack 的 Java 长期支持

Azure 和 Azure Stack 的 Java 开发人员可以使用 [Azul Zulu for Azure - Enterprise Edition](https://www.azul.com/downloads/azure-only/zulu/) JDK 内部版生成并运行生产性 Java 应用程序，而不会产生额外的支持费用。 可以在 Azure 上根据需要使用任何 Java 运行时，但在使用 Zulu 时，可以获得免费的维护更新，还可以向 Microsoft 提交支持问题。

> [!div class="nextstepaction"]
> [下载并安装 Java](java-jdk-install.md)

## <a name="long-term-support-lts"></a>长期支持 (LTS)

* [Java 11](https://www.azul.com/downloads/azure-only/zulu/?&version=java-11-lts)
* [Java 8](https://www.azul.com/downloads/azure-only/zulu/?&version=java-8-lts)
* [Java 7](https://www.azul.com/downloads/azure-only/zulu/?&version=java-7-lts)

## <a name="technical-preview"></a>Technical Preview

* [Java 13](https://www.azul.com/downloads/azure-only/zulu/?&version=java-13)

## <a name="what-is-the-zulu-openjdk-for-azure"></a>适用于 Azure 的 Zulu OpenJDK 是什么？

Azul Zulu for Azure - Enterprise Edition 内部版 OpenJDK 是适用于 Azure 和 Azure Stack 的 OpenJDK 的免费、多平台、生产就绪型发行版，由 Microsoft 及 Azul Systems 提供支持。 这些发行版具有以下特点：

* 是 OpenJDK 的 100% 开源内部版本，已经打包为 Java 开发工具包 (JDK)、Java Runtime Environment (JRE) 和无头 JRE。 这些二进制文件是 Java Standard Edition (SE) 的完全兼容且合规的商业版本，可以与 Azure 和 Azure Stack 上的 Java 应用程序或组件配合使用。
* 与长期支持（包括 Bug 修复、性能增强功能以及安全修补程序）一起提供。
* 用于在 Windows、Linux 和 MacOS 上开发和运行 Java 应用程序。
* 在 Docker Hub 上以容器映像的形式提供，在 Azure 市场上以虚拟机（Windows 和 Linux）的形式提供。
* 由 Microsoft Azure 用来支持许多 Azure 服务，例如：
  * 应用服务 Windows
  * 应用服务 Linux
  * 函数
  * Service Fabric
  * HDInsight
  * 搜索
  * Azure DevOps
  * Cloud Shell  

## <a name="supported-java-versions-and-update-schedule"></a>支持的 Java 版本和更新计划

Azul Systems 为 Java 的所有长期支持 (LTS) 版本（从 Java SE 7、8、11 开始）提供完全支持的 [Azul Zulu for Azure - Enterprise Edition](https://www.azul.com/downloads/azure-only/zulu/) 内部版。 详见 [Azul 新闻稿](https://www.azul.com/press_release/free-java-production-support-for-microsoft-azure-azure-stack)。

|Java SE LTS  |支持截止时间  |
|---------|----------|
|[![支持的 Java 版本 - Java 7](media/supported-java-versions-java-7.png)](https://www.azul.com/downloads/azure-only/zulu/?&version=java-7-lts) |2023 年 7 月 |
|[![支持的 Java 版本 - Java 8](media/supported-java-versions-java-8.png)](https://www.azul.com/downloads/azure-only/zulu/?&version=java-8-lts) |2025 年 3 月|
|[![支持的 Java 版本 - Java 11](media/supported-java-versions-java-11.png)](https://www.azul.com/downloads/azure-only/zulu/?&version=java-11-lts) |2026 年 9 月|
|[![支持的 Java 版本 - Java 13](media/supported-java-versions-java-13.png)](https://www.azul.com/downloads/azure-only/zulu/?&version=java-13) |**预览**|

这些 JDK 发布版本有季度安全更新和 Bug 修复，并根据需要提供关键的带外更新和修补程序。  此支持包括后向移植在新版 Java（例如 Java 11）中报告的针对 Java 7 和 8 的安全更新和 Bug 修复，确保旧版 Java 的持续稳定性和安全性。  Azure 客户可以获取这些安全更新和平台 Bug 修复，不需支付任何计划外 Java SE 订阅费用。

Azul Systems 为这些版本保留了一个 [Java SE 路线图](https://www.azul.com/products/azul_support_roadmap/)。

## <a name="benefits-for-developers"></a>为开发人员带来的好处

Azul Zulu for Azure - Enterprise Edition JDK 发布版具有以下特点：

1. 由 Microsoft 和 Azul Systems 提供支持

   * Zulu 二进制文件已做好生产准备，由 Microsoft 和 Azul Systems 提供支持
   * Zulu 为 Java 7、8、11 提供免费长期支持 (LTS)。 （还将为 Java 17 提供 LTS）。 可以只在需要的时候升级 Java 版本。
   * Java 7 的支持截止时间是 2023 年 7 月。 在 2024 年之后仍会支持 Java 8 和 11。
   * Microsoft 致力于在为许多 Azure 服务提供支持的计算机上以内部方式运行 Zulu。

2. 生产就绪

   * 100% 开源（就其内部版 OpenJDK 来说）。
   * 可随时替换许多 Java SE 发行版。
   * JDK、JRE 和 JRE-headless
   * Java 7、8 和 11
   * 经 OpenJDK 社区技术兼容性工具包 (TCK) 验证符合 Java SE 规范。
   * 开发人员会继续收到 Java SE 的生产更新，包括 Java SE 7、8 和 11 的 Bug 修复、性能增强功能以及安全修补程序。

3. 多平台支持。 Zulu 支持的二进制文件适用于多个平台和版本，包括：

   * Windows 客户端
     * 10
     * 8.1
     * 8、7
   * Windows Server
     * 2016R2
     * 2016
     * 2012 R2
     * 2012
     * 2008 R2
   * Linux，包括
     * RHEL
     * CentOS
     * Ubuntu
     * SLES
     * Debian
     * Oracle Linux
   * Mac OS X
   * 提供多种包类型：
     * MSI、ZIP、TAR、DEB、RPM 和 DMG

    Docker 上提供适用于 Zulu JDK、JRE 和 JRE-headless 且基于多个基 OS 映像的认证 Docker 容器映像。

    中心：

    * [JDK](https://hub.docker.com/_/microsoft-java-jdk)
    * [JRE](https://hub.docker.com/_/microsoft-java-jre)
    * [JRE-headless](https://hub.docker.com/_/microsoft-java-jre-headless)

4. 免费

   * Microsoft 免费提供在 Azure 上生成和缩放 Java 应用所需的一切。 你可以通过 Zulu 接收针对 Java 应用的免费安全更新和平台 Bug 修复，不需任何费用。
   * Zulu Java 8、11 和 12（预览版）提供 [Java Flight Recorder 和 Mission Control](java-jdk-flight-recorder-and-mission-control.md)。

5. 非 LTS 版本的技术预览

   * 可以通过技术预览以渐进方式对短期版本中提供的新功能进行测试，这些短期版本最终会成为 Java 17 LTS。

6. 对 OpenJDK 的更改将向上游传播

   * Azul Systems 提交者会推送针对 OpenJDK 的 Zulu 更改，充实上游存储库。

Java 开发人员可以一如既往地将自己的 Java 运行时（包括 Oracle JDK 和 Red Hat JDK）引入 Azure，并使用安全的基础结构和功能丰富的服务。 Oracle Java SE 的生产版本也可供在 Azure 上的 Windows 或 Linux 虚拟机中运行 Java 工作负荷的 Java 开发人员使用。

## <a name="use-for-local-development"></a>用于本地开发

开发人员可以[下载](https://www.azul.com/downloads/azure-only/zulu/)用于 Azure 和 Azure Stack 的 Java JDK，以便在本地开发环境中使用。 下载内容适用于 Windows、Linux 和 macOS。 在 Linux 上工作的开发人员也可通过 [yum](https://www.azul.com/downloads/azure-only/zulu/#yum-repo) 和 [apt](https://www.azul.com/downloads/azure-only/zulu/#apt-repo) 包管理器获取包。

如需其他指南，请参阅 [Azure 的 Docker 映像](java-jdk-docker-images.md)。