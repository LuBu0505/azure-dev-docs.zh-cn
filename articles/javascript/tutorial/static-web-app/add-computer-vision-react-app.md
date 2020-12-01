---
title: 使用计算机视觉的 React 代码
description: 本示例包含将计算机视觉添加到 React 应用所需的所有代码。 本教程的这一部分查看步骤和代码。
ms.topic: tutorial
ms.date: 11/13/2020
ms.custom: devx-track-js
ms.openlocfilehash: a246e2e367aef4027516468ae691aa31117cd6c7
ms.sourcegitcommit: 4dac39849ba2e48034ecc91ef578d11aab796e58
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/20/2020
ms.locfileid: "94993418"
---
# <a name="5-review-how-to-add-computer-vision-to-the-react-app"></a>5.查看如何将计算机视觉添加到 React 应用

本示例包含将计算机视觉添加到 React 应用所需的所有代码。 本教程的这一部分查看步骤和代码。 对于本教程，无需执行以下步骤。 

## <a name="add-computer-vision-to-local-react-app"></a>将计算机视觉添加到本地 React 应用

使用 npm 将计算机视觉添加到 package.json 文件。 

```bash
npm install @azure/cognitiveservices-computervision 
```

## <a name="add-computer-vision-code-as-separate-module"></a>将计算机视觉代码作为单独的模块添加

计算机视觉代码包含在名为 `./src/VisualAI.js` 的单独的文件中。 重点介绍模块的主要功能。 

:::code language="javascript" source="~/../js-e2e-client-cognitive-services/src/VisualAI.js" highlight="55-75" :::

## <a name="add-catalog-of-images-as-separate-module"></a>将图像目录作为单独的模块添加

如果用户未输入图像 URL，则应用会从目录中随机选择一个图像。 重点介绍随机选择功能 

:::code language="javascript" source="~/../js-e2e-client-cognitive-services/src/DefaultImages.js" highlight="33-35" :::

## <a name="add-custom-computer-vision-module-to-react-app"></a>将自定义计算机视觉模块添加到 React 应用

将方法添加到 React `app.js`。 重点介绍图像分析和结果显示功能。

:::code language="javascript" source="~/../js-e2e-client-cognitive-services/src/App.js" highlight="20-25, 29-42" :::

## <a name="next-step"></a>后续步骤

> [!div class="nextstepaction"]
> [清理资源](clean-up-resources.md) 