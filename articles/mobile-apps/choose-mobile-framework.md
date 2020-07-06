---
title: 选择前端开发平台以通过 Visual Studio 和 Azure 服务构建客户端应用程序
description: 了解用于构建客户端应用程序的受支持本机和跨平台语言。
author: codemillmatt
ms.service: mobile-services
ms.assetid: 355f0959-aa7f-472c-a6c7-9eecea3a34b9
ms.topic: conceptual
ms.date: 06/08/2020
ms.author: masoucou
ms.openlocfilehash: 8972c2b24334c964e00f5a3ccc27fa0c99e6f597
ms.sourcegitcommit: f02ff55cef47581303a217cf1d310879cd722237
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/09/2020
ms.locfileid: "84632479"
---
# <a name="choose-a-mobile-development-framework"></a>选择移动开发框架

开发人员可针对跨平台方法使用特定框架和模式，借助客户端技术自行构建移动应用程序。 开发人员可以构建以下应用程序，具体取决于其决定：

- 使用 Objective C 和 Java 等语言构建本机单平台应用程序
- 使用 Xamarin、.NET 和 C# 构建跨平台应用程序
- 使用 Cordova 及其变体构建混合应用程序

## <a name="native-platforms"></a>本机平台

构建本机应用程序需要使用特定于平台的编程语言、SDK、开发环境以及操作系统供应商提供的其他工具。

### <a name="ios"></a>iOS

iOS 由 Apple 创建和开发，用于在 Apple 设备（即 iPhone 和 iPad）上构建应用。

- **编程语言**：Objective-C、Swift
- **IDE**：Xcode
- **SDK**：iOS SDK

### <a name="android"></a>Android

Android 由 Google 设计，是世界上最常用的操作系统，用于构建可在众多智能手机和平板电脑上运行的应用程序。

- **编程语言**：Java、Kotlin 
- **IDE**：Android Studio 和 Android 开发人员工具 
- **SDK**：Android SDK

### <a name="windows"></a>Windows

- **编程语言**：C#
- **IDE**：Visual Studio 和 Visual Studio Code
- **SDK**：Windows SDK

#### <a name="native-platform-pros"></a>本机平台优点

- 良好的用户体验
- 具有高性能并可与本机库交互的响应式应用程序
- 高度安全的应用程序

#### <a name="native-platform-cons"></a>本机平台缺点

- 应用程序只在一个平台上运行
- 构建应用程序需要更多的开发人员资源，成本更高
- 代码重用率较低

## <a name="cross-platforms-and-hybrid-applications"></a>跨平台和混合应用程序

借助跨平台应用程序，你可一次编写本机移动应用程序、共享代码并在 iOS、Android 和 Windows 上运行。

### <a name="xamarin"></a>Xamarin

[Xamarin](https://visualstudio.microsoft.com/xamarin/) 由 Microsoft 拥有，用于通过 C# 构建可靠的跨平台移动应用程序。 Xamarin 具有可在多个平台（如 iOS、Android 和 Windows）上运行的类库和运行时。 它还可用于编译提供高性能的本机（非解释）应用程序。 Xamarin 结合了本机平台的所有功能，并自行添加了许多强大的功能。

- **编程语言**：C#
- **IDE**：Windows 或 Mac 上的 Visual Studio

### <a name="react-native"></a>React Native

[React Native](https://facebook.github.io/react-native/) 由 Facebook 于 2015 年发布，是一个[开放源代码](https://github.com/facebook/react-native) JavaScript 框架，用于编写适用于 iOS 和 Android 的真实、本机呈现的移动应用程序。 它基于 React，React 是用于构建用户界面的 Facebook JavaScript 库。 它以移动平台为目标，并非以浏览器为目标。 React Native 使用本机组件作为构建基块，而非 Web 组件。

- **编程语言**：JavaScript
- **IDE**：Visual Studio Code

### <a name="unity"></a>Unity

 Unity 是一个为创建游戏而优化的引擎。 可通过该引擎使用 C# 来创建适用于 Windows、iOS、Android 和 Xbox 等平台的高质量 2D 或 3D 应用程序。

### <a name="cordova"></a>Cordova

Cordova 允许使用 Visual Studio Tools for Apache Cordova 或用于 Cordova 的带有扩展的 Visual Studio Code 来构建混合应用程序。 通过这种混合方法，你可以与网站共享组件，并使用基于 Cordova 的托管 Web 应用程序方法重用基于 Web 服务器的应用程序。

#### <a name="cross-platform-pros"></a>跨平台优点

- 通过为多个平台创建一个代码库来提高代码可用性
- 满足多个平台的更多受众需要
- 大大缩短了开发时间
- 易于启动和更新

#### <a name="cross-platform-cons"></a>跨平台缺点

- 性能较低
- 缺乏灵活性
- 每个平台都具有一组独特的特性和功能，使本机应用程序更具创造性
- 增加了 UI 设计时间
- 工具限制
