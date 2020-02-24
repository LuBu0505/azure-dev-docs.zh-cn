---
title: 将 Java 应用程序迁移到 Azure
description: 本主题概述了关于如何将 Java 应用程序迁移到 Azure 的建议策略。
author: yevster
ms.author: yebronsh
ms.topic: conceptual
ms.date: 1/20/2020
ms.openlocfilehash: 7025393f45a32a42fcd2ae3dafee6274a4ba3cbc
ms.sourcegitcommit: aceed8548ad4529a81d83eb15a095edc8607cac5
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/18/2020
ms.locfileid: "77440858"
---
# <a name="migrate-java-applications-to-azure"></a>将 Java 应用程序迁移到 Azure

本主题概述了关于如何将 Java 应用程序迁移到 Azure 的建议策略。

## <a name="identifying-application-type"></a>确定应用程序类型

在为 Java 应用程序选择云目标之前，需确定其应用程序类型。 大多数 Java 应用程序都是以下类型之一：

* [Spring Boot/JAR 应用程序](#spring-boot--jar-applications)
* [Spring Cloud/微服务](#spring-cloud--microservices)
* [Web 应用程序](#web-applications)
* [Java EE 应用程序](#java-ee-applications)
* [批处理作业/计划的作业](#batch--scheduled-jobs)

将在下面各部分介绍这些类型。

### <a name="spring-boot--jar-applications"></a>Spring Boot/JAR 应用程序

许多更新的应用程序都是从命令行直接调用。 这些应用程序仍处理 Web 请求，但不依赖于应用程序服务器来提供 HTTP 请求处理，而是将 HTTP 通信和所有其他依赖项直接合并到应用程序包中。 此类应用程序通常使用框架（如 Spring Boot、Dropwizard、Micronaut、MicroProfile、Vert.x 等）生成。

这些应用程序会打包成带 *.jar* 扩展的存档（JAR 文件）。

### <a name="spring-cloud--microservices"></a>Spring Cloud/微服务

微服务体系结构样式是一种方法，用于将单个应用程序开发为一系列小型服务，每项服务都在其自己的进程中运行，并通过轻型机制（通常是 HTTP 资源 API）通信。 这些服务围绕业务功能构建，可通过完全自动化的部署机制进行独立部署。 这些服务可能使用不同的编程语言编写，并使用不同的数据存储技术，必须对其进行最低限度的集中管理。 此类服务通常使用框架（如 Spring Cloud）构建。

这些服务会打包成多个带 *.jar* 扩展的应用程序（JAR 文件）。

### <a name="web-applications"></a>Web 应用程序

Web 应用程序在 [Servlet](https://en.wikipedia.org/wiki/Java_servlet) 容器中运行。 某些 Web 应用程序直接使用 servlet API，而许多 Web 应用程序则使用其他框架来封装 servlet API，例如 Apache Struts、Spring MVC、JavaServer Faces (JSF) 等。

Web 应用程序会打包成带 *.war* 扩展的存档（WAR 文件）。

### <a name="java-ee-applications"></a>Java EE 应用程序

Java EE 应用程序（也称为 J2EE 应用程序，最新的称为 Jakarta EE 应用程序）可能包含 Web 应用程序的部分或全部元素，或不包含其中的任何元素。 它们还可能包含并使用 [Java EE 规范](https://en.wikipedia.org/wiki/Java_Platform,_Enterprise_Edition)定义的多个其他组件。

Java EE 应用程序可打包为带 *.ear* 扩展的存档（EAR 文件），或带 *.war* 扩展的存档（WAR 文件）。

Java EE 应用程序必须部署到符合 Java EE 规范的应用程序服务器（例如，WebLogic、WebSphere、WildFly、GlassFish、Payara 等）上。

仅依赖于 Java EE 规范提供的功能的应用程序（即，与应用服务器无关的应用程序）可以从一个合规的应用程序服务器迁移到另一个合规的应用程序服务器。 如果应用程序依赖于特定的应用程序服务器（依赖于应用服务器），则可能需要选择一个允许你托管该应用程序服务器的 Azure 服务目标。

### <a name="batch--scheduled-jobs"></a>批处理作业/计划的作业

某些应用程序适用于短暂运行，执行特定工作负载，然后退出，而不是等待请求或用户输入。 此类作业有时需运行一次，有时则需定期按计划的时间间隔运行。 在本地，通常从服务器的 crontab 调用此类作业。

这些应用程序会打包成带 *.jar* 扩展的存档（JAR 文件）。

> [!NOTE]
> 如果应用程序使用计划程序（如 Spring Batch 或 Quartz）来运行计划任务，强烈建议你将此类任务构造为在应用程序外部运行。 如果应用程序在云中扩展为多个实例，则同一作业会运行多次。 此外，如果计划机制使用主机的本地时区，则在跨区域缩放应用程序时可能会遇到意外的行为。

## <a name="selecting-the-target-azure-service-destination"></a>选择针对的 Azure 服务目标

以下部分说明哪些服务目标满足应用程序要求，以及它们涉及的责任。

### <a name="feature-grid"></a>功能网格

请使用以下网格来确定那些支持所需应用程序类型和功能的目标。

|   |应用<br>服务<br>Java SE|应用<br>服务<br>Tomcat|应用<br>服务<br>WildFly|Azure<br>Spring<br>云|AKS|虚拟机|
|---|---|---|---|---|---|---|
| Spring Boot/JAR 应用程序                                    |&#x2714;|        |        |        |&#x2714;|&#x2714;|
| Spring Cloud/微服务                                      |        |        |        |&#x2714;|&#x2714;|&#x2714;|
| Web 应用程序                                                  |        |&#x2714;|&#x2714;|        |&#x2714;|&#x2714;|
| Java EE 应用程序                                              |        |        |&#x2714;|        |&#x2714;|&#x2714;|
| 商业应用程序服务器<br>（例如 WebLogic 或 WebSphere） |        |        |        |        |&#x2714;|&#x2714;|
| 本地文件系统上的长期持久性                         |&#x2714;|&#x2714;|&#x2714;|        |&#x2714;|&#x2714;|
| 应用程序服务器级聚类分析                               |        |        |        |        |&#x2714;|&#x2714;|
| 批处理作业/计划的作业                                            |        |        |        |&#x2714;|&#x2714;|&#x2714;|

### <a name="ongoing-responsibility-grid"></a>持续责任网格

请使用以下网格来了解迁移后每个目标施加给团队的责任。

团队对使用“&#x1F449;”指示的任务持续承担责任。 建议你通过一个稳定且高度自动化的过程来履行所有此类责任。 

> [!NOTE]
> 这并不是一个详尽的责任列表。

|   | 应用服务 | Azure Spring Cloud | AKS | 虚拟机 |
|---|---|---|---|---|
| 更新库<br>（包括漏洞修正）                 | &#x1F449;   | &#x1F449;   | &#x1F449;   | &#x1F449; |
| 更新应用程序服务器<br>（包括漏洞修正）    | ![Azure][1] | ![Azure][1] | &#x1F449;   | &#x1F449; |
| 更新 Java 运行时<br>（包括漏洞修正）          | ![Azure][1] | ![Azure][1] | &#x1F449;   | &#x1F449; |
| 触发 Kubernetes 更新<br>（由 Azure 通过手动触发器执行） | 空值         | 空值         | &#x1F449;   | 空值       |
| 协调非后向兼容的 Kubernetes API 变更                  | 空值         | 空值         | &#x1F449;   | 空值       |
| 更新容器基础映像<br>（包括漏洞修正）      | 空值         | 空值         | &#x1F449;   | 空值       |
| 更新操作系统<br>（包括漏洞修正）      | ![Azure][1] | ![Azure][1] | ![Azure][1] | &#x1F449; |
| 检测和重启失败的实例                                   | ![Azure][1] | ![Azure][1] | ![Azure][1] | &#x1F449; |
| 针对更新实施清空和滚动重启                       | ![Azure][1] | ![Azure][1] | ![Azure][1] | &#x1F449; |
| 基础结构管理                                                   | ![Azure][1] | ![Azure][1] | &#x1F449;   | &#x1F449; |
| 监视和警报管理                                             | &#x1F449;   | &#x1F449;   | &#x1F449;   | &#x1F449; |

如果将 servlet 容器（如 Spring Boot）作为应用程序的一部分进行部署，则应将其视为一个库。因此，这始终是你的责任。

## <a name="ensuring-on-premises-connectivity"></a>确保本地连接性

如果应用程序需要访问任何本地服务，则需预配 Azure 的某个连接服务。 有关详细信息，请参阅[选择用于将本地网络连接到 Azure 的解决方案](/azure/architecture/reference-architectures/hybrid-networking/)。 或者，你需要重构应用程序，以便使用本地资源公开的公开可用的 API。

在开始任何迁移之前，应完成此操作。

## <a name="inventory-current-capacity-and-resource-usage"></a>清点当前容量和资源使用情况

记录当前生产服务器的硬件，以及平均和峰值请求计数和资源使用情况。 将需要此信息来预配服务目标中的资源。

## <a name="migration-guidance"></a>迁移指导

请使用以下网格，按应用程序类型和针对的 Azure 服务目标来查找迁移指南。

**Java 应用程序**

请使用以下行查找 Java 应用程序类型和列，找出将托管应用程序的 Azure 服务目标。

|目标&nbsp;→<br><br>应用程序&nbsp;类型&nbsp;↓|应用<br>服务<br>Java SE|应用<br>服务<br>Tomcat|应用<br>服务<br>WildFly|Azure<br>Spring<br>云|AKS|虚拟机|
|---|---|---|---|---|---|---|
| Spring Boot/<br>JAR 应用程序 | [可用][5] | 已计划        | 已计划 | 已计划 | 已计划        | 已计划 |
| Spring Cloud/<br>微服务   | 空值            | 空值            | 空值     | 已计划 | 已计划        | 已计划 |
| Web 应用程序<br>Web 应用程序     | 空值            | [可用][2] | 空值     | 空值     | [可用][3] | 已计划 |

**Java EE 应用程序**

请使用以下行查找在特定应用服务器上运行的 Java EE 应用程序类型。 请使用列来查找将托管应用程序的 Azure 服务目标。

|目标&nbsp;→<br><br>应用服务器&nbsp;↓|应用<br>服务<br>Java SE|应用<br>服务<br>Tomcat|应用<br>服务<br>WildFly|Azure<br>Spring<br>云|AKS|虚拟机|
|---|---|---|---|---|---|---|
| WildFly/<br>JBoss AS | 空值 | 空值 | 已计划 | 空值 | 已计划 | 已计划        |
| WebLogic              | 空值 | 空值 | 已计划 | 空值 | 已计划 | [可用][4] |
| WebSphere             | 空值 | 空值 | 已计划 | 空值 | 已计划 | 已计划        |
| JBoss EAP             | 空值 | 空值 | 已计划 | 空值 | 空值     | 已计划        |

<!-- reference links, for use with tables -->
[1]: media/migration-overview/logo_azure.svg
[2]: migrate-tomcat-to-tomcat-app-service.md
[3]: migrate-tomcat-to-containers-on-azure-kubernetes-service.md
[4]: migrate-weblogic-to-virtual-machines.md
[5]: migrate-java-se-to-java-se-app-service.md
