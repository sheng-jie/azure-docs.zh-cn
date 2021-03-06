---
title: 收集 Azure Sentinel 预览版中的 Azure AD Identity Protection 数据 |Microsoft Docs
description: 了解如何收集 Azure Sentinel 中的 Azure AD Identity Protection 数据。
services: sentinel
documentationcenter: na
author: rkarlin
manager: barbkess
editor: ''
ms.assetid: 91c870e5-2669-437f-9896-ee6c7fe1d51d
ms.service: sentinel
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 2/28/2019
ms.author: rkarlin
ms.openlocfilehash: 609aced38b7e30f78d81934867196c568dcc85ca
ms.sourcegitcommit: ad019f9b57c7f99652ee665b25b8fef5cd54054d
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/02/2019
ms.locfileid: "57239992"
---
# <a name="collect-data-from-azure-ad-identity-protection"></a>从 Azure AD Identity Protection 收集数据

> [!IMPORTANT]
> Azure Sentinel 目前处于公共预览状态。
> 此预览版在提供时没有附带服务级别协议，不建议将其用于生产工作负荷。 某些功能可能不受支持或者受限。 有关详细信息，请参阅 [Microsoft Azure 预览版补充使用条款](https://azure.microsoft.com/support/legal/preview-supplemental-terms/)。

可以从日志流式传输[Azure AD Identity Protection](https://docs.microsoft.com/azure/information-protection/reports-aip)到 Azure Sentinel 流到 Azure Sentinel 若要查看的仪表板、 创建自定义警报和改进的调查的警报。 Azure Active Directory Identity Protection 提供存在风险的用户、 风险事件和漏洞的综合的视图，能够立即修复风险，并设置策略以自动修复以后的事件。 该服务基于 Microsoft 保护用户身份的经验，并获得超过一天的 13 亿个日志的项的信号的准确性。 


## <a name="prerequisites"></a>必备组件

- 您必须具有[Azure Active Directory Premium P1 或 P2 许可证](https://azure.microsoft.com/pricing/details/active-directory/)
- 具有全局管理员或安全管理员权限的用户


## <a name="connect-to-azure-ad-identity-protection"></a>连接到 Azure AD Identity Protection

如果已有 Azure AD Identity Protection，请确保它是[在网络上启用](../active-directory/identity-protection/enable.md)。
如果 Azure AD Identity Protection 已部署且获取数据，警报数据可以轻松地进行流式传输到 Azure Sentinel。


1. 在 Azure Sentinel，选择**数据收集**，然后单击**Azure AD Identity Protection**磁贴。

2. 单击**Connect**开始 Azure AD Identity Protection 事件传输到 Azure Sentinel。


6. 若要使用 Log Analytics 中的 Azure AD Identity Protection 警报相关的架构，搜索**IdentityProtectionLogs_CL**。

## <a name="next-steps"></a>后续步骤
在本文档中，您学习了如何将 Azure AD Identity Protection 连接到 Azure Sentinel。 若要了解有关 Azure Sentinel 的详细信息，请参阅以下文章：
- 了解如何[来了解一下你的数据和潜在威胁](quickstart-get-visibility.md)。
- 开始[检测威胁 Azure Sentinel](tutorial-detect-threats.md)。
