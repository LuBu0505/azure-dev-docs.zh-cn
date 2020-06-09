---
title: Azure 上适用于 Python 应用的消息和 IoT 入门
description: Azure 文档中适用于 Python 应用的消息和 IoT 的入门材料索引。
ms.date: 05/28/2020
ms.topic: conceptual
ms.openlocfilehash: 23c4f8d92d8ae0653eff43c29db8d06b15df4e67
ms.sourcegitcommit: a9b9157bb3a802ecfe3699854788d010a3f08d7e
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/29/2020
ms.locfileid: "84207620"
---
# <a name="messaging-and-iot-for-python-apps-on-azure"></a>Azure 上适用于 Python 应用的消息和 IoT

以下文章可帮助你开始使用 Azure 上各种不同的消息选项。

## <a name="messaging"></a>消息传递

- **通知**：
  - [如何通过 Python 使用通知中心](/azure/notification-hubs/notification-hubs-python-push-notification-tutorial)

- 队列：
  - [如何通过 Python 使用 Azure 队列存储 v2.1](/azure/storage/queues/storage-python-how-to-use-queue-storage)
  - [适用于 Python 的 Azure 队列存储客户端库 v12](/azure/storage/queues/storage-quickstart-queues-python)
  - [通过 Python 使用 Azure 服务总线队列](/azure/service-bus-messaging/service-bus-python-how-to-use-queues)
  - [通过 Python 使用服务总线主题和订阅](/azure/service-bus-messaging/service-bus-python-how-to-use-topics-subscriptions)

- **实时 Web 功能 (SignalR)** ：
  - [使用 Python 通过 Azure Functions 和 SignalR 服务创建聊天室](/azure/azure-signalr/signalr-quickstart-azure-functions-python)

## <a name="event-ingestion"></a>事件引入

- **事件引入**：
  - [使用 Python 通过事件中心引入实时数据](/azure/event-hubs/event-hubs-python)
  - [使用 Python 在 Azure 存储中捕获事件中心数据并读取该数据](/azure/event-hubs/get-started-capture-python-v2)
  - [使用 Azure CLI 和事件网格将自定义事件路由到 Web 终结点](/azure/event-grid/custom-event-quickstart)

## <a name="internet-of-things-iot"></a>物联网 (IoT)

- **IoT 中心**：
  - [将遥测数据从设备发送到 IoT 中心并使用后端应用程序读取该数据](/azure/iot-hub/quickstart-send-telemetry-python)
  - [使用 IoT 中心发送云到设备的消息](/azure/iot-hub/iot-hub-python-python-c2d)
  - [通过 IoT 中心将设备中的文件上传到云](/azure/iot-hub/iot-hub-python-python-file-upload)
  - [计划和广播作业](/azure/iot-hub/iot-hub-python-python-schedule-jobs)
  - [控制连接到 IoT 中心的设备](/azure/iot-hub/quickstart-control-device-python)

- **设备预配**：
  - [创建和预配模拟的 TPM 设备](/azure/iot-dps/quick-create-simulated-device-tpm-python)
  - [将 TPM 设备注册到 IoT 中心设备预配服务](/azure/iot-dps/quick-enroll-device-tpm-python)
  - [创建和预配模拟的 X.509 设备](/azure/iot-dps/quick-create-simulated-device-x509-python)
  - [将 X.509 设备注册到设备预配服务](/azure/iot-dps/quick-enroll-device-x509-python)

- **IoT Central/IoT Edge**：
  - [教程：创建客户端应用程序并将其连接到 Azure IoT Central 应用程序 (Python)](/azure/iot-central/core/tutorial-connect-device-python)
  - [教程：为 Linux 设备开发和部署 Python IoT Edge 模块](/azure/iot-edge/tutorial-python-module)
