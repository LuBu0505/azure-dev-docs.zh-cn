---
author: edburns
ms.author: edburns
ms.date: 1/21/2020
ms.openlocfilehash: 73f3893f22764280ed5d7c030e7c8eab7d2bf6b1
ms.sourcegitcommit: 226ebca0d0e3b918928f58a3a7127be49e4aca87
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/08/2020
ms.locfileid: "82988796"
---
如果应用程序包含的代码有主机 OS 的依赖项，则需重构该代码，删除那些依赖项。 例如，可能需要将文件系统路径中使用的 `/` 或 `\` 替换为 [`File.Separator`](https://docs.oracle.com/javase/8/docs/api/java/io/File.html#separator) 或 [`Paths.get`](https://docs.oracle.com/javase/8/docs/api/java/nio/file/Paths.html#get-java.lang.String-java.lang.String...-)。
