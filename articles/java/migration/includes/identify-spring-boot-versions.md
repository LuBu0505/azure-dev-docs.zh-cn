---
author: yevster
ms.author: yebronsh
ms.date: 4/15/2020
ms.openlocfilehash: 34701a7d3fd85fea412be859dc21823df2dde053
ms.sourcegitcommit: 226ebca0d0e3b918928f58a3a7127be49e4aca87
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/08/2020
ms.locfileid: "82990189"
---
#### <a name="identify-spring-boot-versions"></a>确定 Spring Boot 版本

检查要迁移的每个应用程序的依赖项，以确定其 Spring Boot 版本。

##### <a name="maven"></a>Maven

在 Maven 项目中，通常可以在 POM 文件的 `<parent>` 元素中找到 Spring Boot 版本：

```xml
    <parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>2.2.6.RELEASE</version>
        <relativePath/> <!-- lookup parent from repository -->
    </parent>
```

##### <a name="gradle"></a>Gradle

在 Gradle 项目中，通常可以在 `plugins` 部分找到 Spring Boot 版本（作为 `org.springframework.boot` 插件的版本）：

```gradle
plugins {
  id 'org.springframework.boot' version '2.2.6.RELEASE'
  id 'io.spring.dependency-management' version '1.0.9.RELEASE'
  id 'java'
}
```
