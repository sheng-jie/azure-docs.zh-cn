---
title: 了解 Azure Service Fabric 支持选项 | Microsoft Docs
description: 支持的 Azure Service Fabric 群集版本，以及文件支持票证的链接
services: service-fabric
documentationcenter: .net
author: pkcsf
manager: jpconnock
editor: ''
ms.assetid: ''
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: troubleshooting
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 8/24/2018
ms.author: pkc
ms.openlocfilehash: aa4fd391aef3acf8e72513d91b62c7f16a13062d
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/18/2019
ms.locfileid: "57889431"
---
# <a name="azure-service-fabric-support-options"></a>Azure Service Fabric 支持选项

我们为用户设置了各种选项，方便其为 Service Fabric 群集（在其上运行应用程序工作负荷）提供相应的支持。 用户需根据所需支持级别以及问题的严重性，选取适当的选项。 

## <a name="report-production-issues-or-request-paid-support-for-azure"></a>报告生产问题，或者请求 Azure 付费支持

若要报告部署在 Azure 上的 Service Fabric 群集的问题，请通过 [Azure 门户](https://ms.portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/overview)或 [Microsoft 支持门户](https://support.microsoft.com/oas/default.aspx?prid=16146)开具支持票证。

了解有关以下方面的详细信息：
 
- [Microsoft 提供的 Azure 支持](https://azure.microsoft.com/support/plans/?b=16.44)。
- [Microsoft 顶级支持](https://support.microsoft.com/en-us/premier)。

<a id="getlivesitesupportonprem"></a>

## <a name="report-production-issues-or-request-paid-support-for-standalone-service-fabric-clusters"></a>报告生产问题，或者请求独立 Service Fabric 群集的付费支持

若要报告部署在本地或其他云上的 Service Fabric 群集的问题，请通过 [Microsoft 支持门户](https://support.microsoft.com/oas/default.aspx?prid=16146)开具专业支持票证。

了解有关以下方面的详细信息：

- [Microsoft 提供的本地专业支持](https://support.microsoft.com/en-us/gp/offerprophone?wa=wsignin1.0)。
- [Microsoft 顶级支持](https://support.microsoft.com/en-us/premier)。

## <a name="report-azure-service-fabric-issues"></a>报告 Azure Service Fabric 问题

我们已设置 GitHub 存储库，用于报告 Service Fabric 问题。  我们还积极监视以下论坛。

### <a name="github-repo"></a>GitHub 存储库 

在 [Service-Fabric-issues git 存储库](https://github.com/Azure/service-fabric-issues)中报告 Azure Service Fabric 问题。 此存储库用于报告和跟踪 Azure Service Fabric 问题，以及进行小型功能请求。 **请勿使用此功能报告实时站点问题**。

### <a name="stackoverflow-and-msdn-forums"></a>StackOverflow 和 MSDN 论坛

[StackOverflow 上的 Service Fabric 标记][stackoverflow]和 [MSDN 上的 Service Fabric 论坛][msdn-forum]最适合提问有关平台工作方式以及如何通过该平台完成某些任务的问题。

### <a name="azure-feedback-forum"></a>Azure 反馈论坛

[有关 Service Fabric 的 Azure 反馈论坛][uservoice-forum]最适合提交用户关于产品的大型功能创意，我们可以看到，大多数常见的请求都属于我们的中长期规划。 我们鼓励你在社区内争取大家对建议的支持。

## <a name="service-fabric-preview-versions---unsupported-for-production-use"></a>Service Fabric 预览版本 - 不支持在生产环境中使用

我们会不时发布包含重要功能的版本，希望用户对这些功能提供反馈，这些版本将作为预览版发布。 这些预览版本应仅用于测试目的。 生产群集应始终运行支持的稳定 Service Fabric 版本。 预览版本始终以主版本号和次版本号 255 开头。 例如，如果看到 Service Fabric 版本 255.255.5703.949，则该版本应仅在测试群集中使用且处于预览状态。 这些预览版本也在 [Service Fabric 团队博客](https://blogs.msdn.microsoft.com/azureservicefabric)上公布，并将提供有关包含的功能的详细信息。
这些预览版本没有付费的支持选项。 使用[报告 Azure Service Fabric 问题](https://docs.microsoft.com/azure/service-fabric/service-fabric-support#report-azure-service-fabric-issues)下列出的选项之一提出问题或提供反馈。

## <a name="next-steps"></a>后续步骤

[支持的 Service Fabric 版本](service-fabric-versions.md)

<!--references-->
[msdn-forum]: https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureServiceFabric
[stackoverflow]: https://stackoverflow.com/questions/tagged/azure-service-fabric
[uservoice-forum]: https://feedback.azure.com/forums/293901-service-fabric
[acom-docs]: https://aka.ms/servicefabricdocs
[sample-repos]: https://aka.ms/servicefabricsamples