---
title: 将变量替换与适用于 Azure 的 GitHub Actions 一起使用
description: 用于替换参数化文件中的变量的 GitHub 操作
author: juliakm
ms.author: jukullam
ms.topic: conceptual
ms.service: azure
ms.date: 11/18/2020
ms.custom: github-actions-azure
ms.openlocfilehash: 0e3f3b11980c987ef4f7a288380b9517ad88d777
ms.sourcegitcommit: 418e446e6ada5d50df283401df4f6b6370a356b9
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/25/2020
ms.locfileid: "96120541"
---
# <a name="use-variable-substitution-with-github-actions"></a>将变量替换与 GitHub Actions 一起使用

了解如何使用[变量替换操作](https://github.com/marketplace/actions/variable-substitution)来替换基于 XML、JSON 和 YAML 的配置和参数文件中的值。

使用变量替换，可以在工作流运行过程中将值（包括 [GitHub 机密](https://docs.github.com/en/free-pro-team@latest/actions/reference/encrypted-secrets)）插入存储库中的文件。 例如，可以在工作流运行过程中将 API 登录名和密码插入 JSON 文件。

变量替换仅适用于对象层次结构中预定义的密钥。 不能使用变量替换创建新密钥。 此外，只能将在工作流中定义为[环境变量](https://docs.github.com/en/free-pro-team@latest/actions/reference/environment-variables)的变量或已可用的系统变量用于替换。

## <a name="prerequisites"></a>先决条件

- 一个 GitHub 帐户。 如果没有该帐户，请注册[免费版](https://github.com/join)。  

## <a name="use-the-variable-substitution-action"></a>使用变量替换操作

此示例演示如何使用[变量替换操作](https://github.com/marketplace/actions/variable-substitution)替换 `employee.json` 中的值。

1. 在存储库的根级别创建 `employee.json`。

    ```json
    {
        "first-name": "Toni",
        "last-name": "Cranz",
        "username": "",
        "password": "",
        "url": ""
    }
    ```

2. 打开 GitHub 存储库并转到“设置”。

    :::image type="content" source="media/github-repo-settings.png" alt-text="在导航中选择“设置”":::

3. 依次选择“机密”和“新建机密” 。

    :::image type="content" source="media/select-secrets.png" alt-text="选择以添加机密":::

4. 添加值为 `5v{W<$2B<GR2=t4#`（或所选密码）的新机密 `PASSWORD`。 保存机密。 

5. 转到“操作”，然后选择“自行设置工作流” 。

6. 添加工作流文件。 JSON 文件中的用户名值将替换为 `tcranz`。 密码将替换为你的 GitHub 机密。 URL 字段将使用包含 GitHub 变量 `github.repository` 的 URL 进行填充。

    ```yaml
    on: [push]
    name: variable substitution in json

    jobs:
    build:
        runs-on: ubuntu-latest
        steps:
        - uses: actions/checkout@v2
        - uses: microsoft/variable-substitution@v1 
        with:
            files: 'employee.json'
        env:
            username: tcranz
            password: ${{ secrets.PASSWORD }}
            url: https://github.com/${{github.repository}}

    ```

7. 转到“操作”，查看工作流运行。 打开变量替换操作。 应会看到每个变量均已替换。

    ```text
    SubstitutingValueonKeyWithString username tcranz
    SubstitutingValueonKeyWithString password ***
    SubstitutingValueonKeyWithString url https://github.com/account/variable-sub
    Successfully updated file: employee.json
    ```

## <a name="clean-up-resources"></a>清理资源

不再需要 GitHub 存储库时，请将其删除。

## <a name="next-steps"></a>后续步骤

> [!div class="nextstepaction"]
> [使用 GitHub Actions 部署到 Azure Web 应用](/azure/app-service/deploy-github-actions)
