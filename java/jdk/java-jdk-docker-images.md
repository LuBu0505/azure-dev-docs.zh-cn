---
title: 将 Docker 映像与 JDK 配合使用，以便进行 Azure Java 开发
description: 了解如何使用命令行接口将 Docker 映像与 Azure 的 Java 开发工具包 (JDK) 结合使用。
author: bmitchell287
manager: douge
ms.author: brendm
ms.date: 04/09/2019
ms.devlang: java
ms.topic: conceptual
ms.service: azure
ms.custom: seo-java-august2019, seo-java-september2019
ms.openlocfilehash: 019ff4764cd57ff5aba1cc6cef5dc1a666cc95a0
ms.sourcegitcommit: 09ecd9676b2f3fa0a30675c89c06b35355f90957
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/22/2019
ms.locfileid: "72776332"
---
# <a name="use-docker-with-a-java-development-kit-jdk-for-azure"></a>为 Azure 将 Docker 与 Java 开发工具包 (JDK) 配合使用 

本文介绍了如何为 Azure 将 Docker 与 Java 开发工具包 (JDK) 配合使用。 针对 Java 7、8、11 预生成的 Docker 映像可以通过 [Docker Hub](https://hub.docker.com/_/microsoft-java-se) 使用。

Docker Hub 上提供适用于 Zulu JDK、JRE 和 JRE-headless 且基于多个基 OS 映像的认证 Docker 容器映像：

* [JDK](https://hub.docker.com/_/microsoft-java-jdk)
* [JRE](https://hub.docker.com/_/microsoft-java-jre)
* [JRE-headless](https://hub.docker.com/_/microsoft-java-jre-headless)

## <a name="running-a-docker-image"></a>运行 Docker 映像

可以通过语法 `$ docker run mcr.microsoft.com/java/jdk:tag java` 运行 Docker 映像，如以下示例所示。

```cli
docker run mcr.microsoft.com/java/jdk:8u212-zulu-alpine java -version 
```

## <a name="creating-a-docker-image"></a>创建 Docker 映像

可以使用 Microsoft 的官方 Docker Hub 映像创建一个映像，如以下示例所示。

### <a name="create-a-docker-file"></a>创建 Docker 文件

```cli
FROM mcr.microsoft.com/java/jdk:8u212-zulu-alpine 
  
RUN echo $' \
  
public class HelloWorld { \
   public static void main(String[] args) { \
      // Prints "Hello, World" in the terminal window. \
      System.out.println("Hello, World - From Microsoft Azure !!!"); \
   } \
}' > HelloWorld.java
  
RUN javac HelloWorld.java
  
CMD ["java", "HelloWorld"]
```

### <a name="build-a-docker-image"></a>生成 Docker 映像

```cli
docker build -t hello-world
```

### <a name="run-the-new-image"></a>运行此新映像

```cli
docker run hello-world
```

将显示以下输出：

```output
Hello World - From Microsoft Azure !!!
```
