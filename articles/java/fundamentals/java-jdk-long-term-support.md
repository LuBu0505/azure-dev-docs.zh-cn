---
title: 面向 Azure 开发的 Java JDK 和中长期支持
description: 本文提供开发和运行 Java 应用程序所需的有关 Azure 支持的下载内容和声明。
ms.date: 04/09/2019
ms.topic: conceptual
ms.custom: seo-java-september2019, devx-track-java
ms.openlocfilehash: 5bffb4e4d2f68ef61ea96ededdf51ea98bb72d2a
ms.sourcegitcommit: 44016b81a15b1625c464e6a7b2bfb55938df20b6
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/14/2020
ms.locfileid: "86379831"
---
# <a name="java-long-term-support-and-medium-term-support-on-azure-and-azure-stack"></a>Azure 和 Azure Stack 上的 Java 长期支持和中期支持

Azure 和 Azure Stack 的 Java 开发人员可以使用 [Azul Zulu for Azure - Enterprise Edition](https://www.azul.com/downloads/azure-only/zulu/) JDK 内部版生成并运行生产性 Java 应用程序，而不会产生额外的支持费用。 可以在 Azure 上使用所需的任何 Java 运行时。 但如果使用 Zulu，可以获得免费的维护更新，还可以向 Microsoft 提交支持票证。

指定为长期支持 (LTS) 的版本与由 Oracle 和 OpenJDK 社区指定的 LTS 版本相同。 对于 LTS 版本，我们根据需要提供至少 8 年的 bug 修复、安全更新和其他（生产支持）修补程序的访问权限。 我们还提供 2 年的额外支持，旨在为迁移到更新 JDK 版本的用户提供建议和帮助（扩展支持）。

对于设计为中期支持 (MTS) 的那些版本，我们将在下一 LTS 版本正式推出后提供至少 1.5 年的生产支持。 我们还额外提供 1 年的扩展支持。

> [!div class="nextstepaction"]
> [下载并安装 Java](java-jdk-install.md)

## <a name="long-term-support-lts"></a>长期支持 (LTS)

* [Java 11](https://www.azul.com/downloads/azure-only/zulu/?&version=java-11-lts)
* [Java 8](https://www.azul.com/downloads/azure-only/zulu/?&version=java-8-lts)
* [Java 7](https://www.azul.com/downloads/azure-only/zulu/?&version=java-7-lts)

## <a name="medium-term-support-mts"></a>中期支持 (MTS)

* [Java 13](https://www.azul.com/downloads/azure-only/zulu/?&version=java-13)

## <a name="technical-preview"></a>Technical Preview

* [Java 14](https://www.azul.com/downloads/azure-only/zulu/?version=java-14)

## <a name="what-is-the-zulu-openjdk-for-azure"></a>适用于 Azure 的 Zulu OpenJDK 是什么？

Azul Zulu for Azure - Enterprise Edition 内部版 OpenJDK 是适用于 Azure 和 Azure Stack 的 OpenJDK 的免费、多平台、生产就绪型发行版。 它们由 Microsoft 及 Azul Systems 提供支持。 这些发行版具有以下特点：

* 是 OpenJDK 的 100% 开放源代码内部版本，已经打包为 Java 开发工具包 (JDK)、Java Runtime Environment (JRE) 和无外设 JRE。 这些二进制文件是 Java Standard Edition (SE) 的完全兼容且合规的商业版本，可以与 Azure 和 Azure Stack 上的 Java 应用程序或组件配合使用。
* 与长期支持（包括 Bug 修复、性能增强功能以及安全修补程序）一起提供。
* 用于在 Windows、Linux 和 macOS 上开发和运行 Java 应用程序。
* 在 Docker Hub 上以容器映像的形式提供，在 Azure 市场上以虚拟机（Windows 和 Linux）的形式提供。
* 由 Microsoft Azure 用来支持许多 Azure 服务，例如：
  * Windows 上的应用服务
  * Linux 上的应用服务
  * Azure Functions
  * Azure Service Fabric
  * Azure HDInsight
  * Azure 认知搜索
  * Azure DevOps
  * Azure Cloud Shell  

## <a name="supported-java-versions-and-update-schedule"></a>支持的 Java 版本和更新计划

Azul Systems 为 Java 的所有长期支持 (LTS) 和中期支持 (MTS) 版本（包括 Java SE 7、8、11、13）提供完全支持的 [Azul Zulu for Azure - Enterprise Edition](https://www.azul.com/downloads/azure-only/zulu/) 内部版。 有关详细信息，请参阅 [Azul 新闻稿](https://www.azul.com/press_release/free-java-production-support-for-microsoft-azure-azure-stack)和 [Azul 产品支持生命周期](https://www.azul.com/products/azul_support_roadmap/)路线图。

|Java SE 版本  |支持截止至  |
|---------|----------|
|[![Java 7 徽标](media/supported-java-versions-java-7.png)](https://www.azul.com/downloads/azure-only/zulu/?&version=java-7-lts) |2023 年 7 月 (LTS)|
|[![Java 8 徽标](media/supported-java-versions-java-8.png)](https://www.azul.com/downloads/azure-only/zulu/?&version=java-8-lts) |2030 年 12 月 (LTS)|
|[![Java 11 徽标](media/supported-java-versions-java-11.png)](https://www.azul.com/downloads/azure-only/zulu/?&version=java-11-lts) |2027 年 9 月 (LTS)|
|[![Java 13 徽标](media/supported-java-versions-java-13.png)](https://www.azul.com/downloads/azure-only/zulu/?&version=java-13) |2023 年 3 月 (MTS)|
|[![Java 14 徽标](media/supported-java-versions-java-14.png)](https://www.azul.com/downloads/azure-only/zulu/?version=java-14) |**预览**|

这些 LTS 和 MTS JDK 版本提供季度安全更新程序和 Bug 修补程序，并根据需要提供关键的带外更新和修补程序。 此支持包括更新版 Java（例如 Java 11）中报告的针对 Java 7 和 8 的安全更新和 Bug 修复的后向移植。 此向后移植可确保旧版 Java 的持续稳定性和安全性。 Azure 客户可以获取这些安全更新和平台 Bug 修复，不需支付任何计划外 Java SE 订阅费用。

目前，Azure Functions 需要使用 Java 8，对 Java 11 的支持仍在开发中。

## <a name="benefits-for-developers"></a>为开发人员带来的好处

Azul Zulu for Azure - Enterprise Edition JDK 版：

- 由 Microsoft 和 Azul Systems 提供支持。

   * Zulu 二进制文件已做好生产准备，由 Microsoft 和 Azul Systems 提供支持。
   * Zulu 为 Java 7、8、11 提供免费长期支持 (LTS)，并为 Java 13 提供中期支持 (MTS)。 （还将为 Java 17 提供 LTS。）可以只在需要的时候升级 Java 版本。
   * Java 7 的支持截止时间是 2023 年 7 月。 对 Java 8 的支持将持续至 2030 年 12 月。 对 Java 11 的支持将持续至 2027 年 9 月。 Java 13 在 2023 年 3 月之前仍会受支持。
   * Microsoft 致力于在为许多 Azure 服务提供支持的计算机上以内部方式运行 Zulu。

- 已做好生产准备。

   * 100% 开源（就其内部版 OpenJDK 来说）。
   * 可随时替换许多 Java SE 发行版。
   * JDK、JRE 和无外设 JRE。
   * Java 7、8、11 和 13。
   * 经 OpenJDK 社区技术兼容性工具包 (TCK) 验证，已符合 Java SE 规范。
   * 包括 Java SE 的生产更新，其中包括 Java SE 7、8、11 和 13 的 Bug 修补程序、性能增强功能以及安全修补程序。

- 具有多平台支持。 Zulu 支持的二进制文件适用于多个平台和版本：

   * Windows
     * 10
     * 8.1
     * 8
     * 7
   * Windows Server
     * 2016 R2
     * 2016
     * 2012 R2
     * 2012
     * 2008 R2
   * Linux
     * RHEL
     * CentOS
     * Ubuntu
     * SLES
     * Debian
     * Oracle Linux
   * Mac OS X
   * 提供多种包类型：
     * MSI、ZIP、tar、deb、RPM 和 DMG

     Docker Hub 上提供适用于 Zulu JDK、JRE 和无外设 JRE 且基于多个基 OS 映像的认证 Docker 容器映像：

     * [JDK](https://hub.docker.com/_/microsoft-java-jdk)
     * [JRE](https://hub.docker.com/_/microsoft-java-jre)
     * [无外设 JRE](https://hub.docker.com/_/microsoft-java-jre-headless)

- 免费。

   * Microsoft 免费提供在 Azure 上生成和缩放 Java 应用所需的一切。 你可以通过 Zulu 接收针对 Java 应用的免费安全更新和平台 Bug 修复。
   * Zulu Java 8、11 和更高版本中提供 [Java Flight Recorder 和 Zulu Mission Control](java-jdk-flight-recorder-and-mission-control.md)。

- 包括非 LTS/MTS 版本的技术预览。

   * 通过技术预览，可以以渐进方式对短期版本中提供的新功能进行测试，这些短期版本最终会进阶为 Java 17 LTS。

- 包含对 OpenJDK 的上游更改。
   * Azul Systems 提交者将 Zulu 更改推送到 OpenJDK。 这些提交内容使上游存储库更综合全面丰富。

Java 开发人员可以一如既往地将自己的 Java 运行时（包括 Oracle JDK 和 Red Hat JDK）引入 Azure，并使用安全的基础结构和功能丰富的服务。 Oracle Java SE 的生产版本也可供在 Azure 上的 Windows 或 Linux 虚拟机中运行 Java 工作负荷的 Java 开发人员使用。

## <a name="use-for-local-development"></a>用于本地开发

开发人员可以[下载用于 Azure 和 Azure Stack 的 Java JDK](https://www.azul.com/downloads/azure-only/zulu/)，以便在本地开发环境中使用。 下载内容适用于 Windows、Linux 和 macOS。 在 Linux 上工作的开发人员也可通过 [yum](https://www.azul.com/downloads/azure-only/zulu/#yum-repo) 和 [apt](https://www.azul.com/downloads/azure-only/zulu/#apt-repo) 包管理器获取包。

有关详细信息，请参阅[适用于 Azure 的 Docker 映像](java-jdk-docker-images.md)。
