---
author: yevster
ms.author: yebronsh
ms.date: 4/15/2020
ms.openlocfilehash: 6fdf908458eec428f4e01d1b5f20fcbf4e039dbe
ms.sourcegitcommit: 226ebca0d0e3b918928f58a3a7127be49e4aca87
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/08/2020
ms.locfileid: "82990219"
---
### <a name="configure-persistent-storage"></a>配置持久性存储

如果应用程序的任何部分读取或写入到本地文件系统，将需要配置持久性存储来替换本地文件系统。 有关详细信息，请参阅[如何在 Azure Spring Cloud 中使用持久性存储](/azure/spring-cloud/spring-cloud-howto-persistent-storage)。

应将任何临时文件写入 `/tmp` 目录。 要实现 OS 独立性，可以使用 `System.getProperty("java.io.tmpdir")` 获取此目录。 还可以使用 [`java.nio.Files::createTempFile`](https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/nio/file/Files.html#createTempFile(java.lang.String,java.lang.String,java.nio.file.attribute.FileAttribute...)) 来创建临时文件。
