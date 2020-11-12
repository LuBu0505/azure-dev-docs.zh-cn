---
title: include 文件 completed.md
description: include 文件 completed.md
ms.topic: include
ms.date: 10/13/2020
ms.custom: devx-track-javascript
ms.openlocfilehash: 09f69293062f52dfc2190cfeb040ad45ea428c67
ms.sourcegitcommit: 801682d3fc9651bf95d44e58574d5a4564be6feb
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/06/2020
ms.locfileid: "94338458"
---
## <a name="install-the-azure-functions-core-tools"></a>安装 Azure Functions Core Tools

若要启用本地调试，需要安装 [Azure Functions Core Tools](https://github.com/Azure/azure-functions-core-tools)，这可以直接在 Visual Studio Code 中完成。

1. 启动 Visual Studio Code。

1. 打开命令面板  ( **F1** )，输入 **Azure Functions:Install or Update Azure Functions Core Tools** ，并按 **Enter** 运行该命令。

1. 若要验证安装，请在 VS Code 中选择菜单命令“终端”   >   “新终端”，然后运行命令 `func`。 该命令应显示如下所示的输出（以及使用情况信息）。

    <pre>
                      %%%%%%
                     %%%%%%
                @   %%%%%%    @
              @@   %%%%%%      @@
           @@@    %%%%%%%%%%%    @@@
         @@      %%%%%%%%%%        @@
           @@         %%%%       @@
             @@      %%%       @@
               @@    %%      @@
                    %%
                    %

    Azure Functions Core Tools (2.4.419 Commit hash: c9c1724d002bd90b2e6b41393915ea3a26bcf0ce)
    Function Runtime Version: 2.0.12332.0
    </pre>