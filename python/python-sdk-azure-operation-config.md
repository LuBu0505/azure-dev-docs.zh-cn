---
title: 用于操作配置的参数 - Azure SDK for Python
description: 用于 Python 的 Azure SDK 引发的 C
author: sptramer
ms.author: sttramer
manager: carmonm
ms.date: 03/07/2018
ms.topic: conceptual
ms.devlang: python
ms.custom: seo-python-october2019
ms.openlocfilehash: 0730cec8470a3b55421c6c0cafa08f88819cb1d8
ms.sourcegitcommit: 6012460ad8d6ff112226b8f9ea6da397ef77712d
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/11/2019
ms.locfileid: "72279086"
---
# <a name="parameters-for-operation-configuration"></a>用于操作配置的参数

可以为 Azure SDK for Python 中操作上的方法提供额外参数。

额外的参数在 `kwargs` 中提供。 此功能称为 *operation_config*。

操作配置选项包括：

|参数名称|类型|角色|
|----------------------|------|---------------|
| 验证 |`bool`|是否要验证 SSL 证书。 默认值为 True。|
|  cert |`str`| 用于客户端验证的本地证书的路径。|
|  timeout |`int`| 用于建立服务器连接的超时值（以秒为单位）。|
|  allow_redirects |`bool` | 是否允许重定向。|
|  max_redirects  |`int`| 允许的最大重定向次数。|
|  proxies  |`dict` |代理服务器设置。|
|  use_env_proxies |`bool` |是否要从本地环境变量读取代理设置。|
|  retries  |`int` | 尝试重试的总次数。|
|  enable_http_logger | `bool`| 以调试模式启用 HTTP 日志（默认情况下为 False）。|
