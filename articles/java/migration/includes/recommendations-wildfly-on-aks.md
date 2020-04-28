---
author: mriem
ms.author: manriem
ms.date: 2/28/2020
ms.openlocfilehash: 1ffc12fa2a99e2abdbbc43e0024afd8e5e98c4b9
ms.sourcegitcommit: 0af39ee9ff27c37ceeeb28ea9d51e32995989591
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/21/2020
ms.locfileid: "81673253"
---
### <a name="recommendations"></a>建议

* 考虑向分配给入口控制器或应用程序负载均衡器的 IP 地址添加 DNS 名称。 有关详细信息，请参阅[在 AKS 中使用静态公共 IP 地址创建入口控制器](/azure/aks/ingress-static-ip)。

* 考虑为应用程序添加 [HELM 图表](https://helm.sh/docs/topics/charts/)。 可以通过 Helm 图表将应用程序部署参数化，供更多样化的客户使用和自定义。

* 设计和实施 DevOps 策略。 若要在提高开发速度的同时保持可靠性，请考虑通过 Azure Pipelines 自动进行部署和测试。 有关详细信息，请参阅[生成并部署到 AKS](/azure/devops/pipelines/ecosystems/kubernetes/aks-template)。

* 为群集启用 Azure 监视。 有关详细信息，请参阅[启用对已部署的 AKS 群集的监视](/azure/azure-monitor/insights/container-insights-enable-existing-clusters)。 这样 Azure Monitor 就可以收集容器日志、跟踪利用率，等等。

* 考虑通过 Prometheus 公开特定于应用程序的指标。 Prometheus 是在 Kubernetes 社区中广泛采用的开源指标框架。 可以配置 Azure Monitor 中的 Prometheus 指标抓取，而不是托管你自己的 Prometheus 服务器，以便进行应用程序指标聚合，并自动响应或升级异常情况。 有关详细信息，请参阅[使用适用于容器的 Azure Monitor 配置 Prometheus 指标的抓取](/azure/azure-monitor/insights/container-insights-prometheus-integration)。

* 设计和实施业务连续性和灾难恢复策略。 对于关键应用程序，请考虑多区域部署体系结构。 有关详细信息，请参阅 [AKS 中的业务连续性和灾难恢复的最佳做法](/azure/aks/operator-best-practices-multi-region)。

* 查看 [Kubernetes 版本支持策略](/azure/aks/supported-kubernetes-versions#kubernetes-version-support-policy)。 你有责任持续更新 AKS 群集，确保其始终运行受支持的版本。 有关详细信息，请参阅[升级 AKS 群集](/azure/aks/upgrade-cluster)。

* 让所有负责群集管理和应用程序开发的团队成员查看相关的 AKS 最佳做法。 有关详细信息，请参阅[群集操作员和开发人员在 AKS 上生成并管理应用程序的最佳做法](/azure/aks/best-practices)。

* 确保部署文件指定滚动更新的完成方式。 有关详细信息，请参阅 Kubernetes 文档中的[滚动更新部署](https://kubernetes.io/docs/concepts/workloads/controllers/deployment/#rolling-update-deployment)。

* 设置自动缩放以处理高峰时间负载。 有关详细信息，请参阅[自动缩放群集以满足 AKS 上的应用程序需求](/azure/aks/cluster-autoscaler)。

* 考虑监视代码缓存大小并在 Dockerfile 中添加 JVM 参数 `-XX:InitialCodeCacheSize` 和 `-XX:ReservedCodeCacheSize`，进一步优化性能。 有关详细信息，请参阅 Oracle 文档中的 [Codecache Tuning](https://docs.oracle.com/javase/8/embedded/develop-apps-platforms/codecache.htm)（Codecache 优化）。
