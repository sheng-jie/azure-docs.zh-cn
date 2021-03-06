---
title: include 文件
description: include 文件
services: networking
author: jimdial
ms.service: networking
ms.topic: include
ms.date: 02/07/2019
ms.author: jdial
ms.custom: include file
ms.openlocfilehash: c6c57390e0a2fba0c79d3198df0f5577eb813f88
ms.sourcegitcommit: bd15a37170e57b651c54d8b194e5a99b5bcfb58f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/07/2019
ms.locfileid: "57553301"
---
<a name="virtual-networking-limits-classic"></a>以下限制仅适用于每个订阅通过经典部署模型托管的网络资源。 了解如何[针对订阅限制查看当前资源使用情况](../articles/networking/check-usage-against-limits.md)。

| 资源 | 默认限制 | 最大限制 |
| --- | --- | --- |
| 虚拟网络 |50 |100 |
| 本地网络站点 |20 |联系支持人员。 |
| 每个虚拟网络的 DNS 服务器 |20 |20 |
| 每个虚拟网络的专用 IP 地址 |4,096 |4,096 |
| 虚拟机或角色实例的单 NIC 并发 TCP 或 UDP 流数 |最多两个或多个 Nic 的 1000000 500,000。 |最多两个或多个 Nic 的 1000000 500,000。 |
| 网络安全组 (NSG) |100 |200 |
| 每个 NSG 的 NSG 规则数 |200 |1,000 |
| 用户定义的路由表 |100 |200 |
| 用户定义的路由每个路由表 |100 |400 |
| 公共 IP 地址 (动态) |5 |联系支持人员。 |
| 保留的公共 IP 地址 |20 |联系支持人员。 |
| 每个部署的公共 VIP |5 |联系支持人员。 |
| 每个部署 （内部负载均衡） 的专用 VIP |第 |第 |
| 终结点访问控制列表 (Acl) |50 |50 |

#### <a name="azure-resource-manager-virtual-networking-limits"></a>网络限制-Azure 资源管理器
以下限制仅适用于每个订阅按区域通过 Azure 资源管理器管理的网络资源。 了解如何[针对订阅限制查看当前资源使用情况](../articles/networking/check-usage-against-limits.md)。

> [!NOTE]
> 我们最近将所有默认限制提高到了最大限制。 如果没有最大限制列，该资源不具有可调整的限制。 如果你在过去的支持通过增加这些限制，看不到下表中的更新的限制[打开免费的联机客户支持请求](../articles/azure-resource-manager/resource-manager-quota-errors.md)

| 资源 | 默认限制 | 
| --- | --- |
| 虚拟网络 |1,000 |
| 每个虚拟网络的子网数 |3,000 |
| 每个虚拟网络的虚拟网络对等互连 |100 |
| 每个虚拟网络的 DNS 服务器 |20 |
| 每个虚拟网络的专用 IP 地址 |65,536 |
| 每个网络接口的专用 IP 地址 |256 |
| 每个虚拟机的专用 IP 地址 |256 |
| 虚拟机或角色实例的单 NIC 并发 TCP 或 UDP 流数 |500,000 |
| 网络接口卡 |65,536 |
| 网络安全组 |5,000 |
| 每个 NSG 的 NSG 规则数 |1,000 |
| 为安全组中的源或目标指定的 IP 地址和范围数 |4,000 |
| 应用程序安全组 |3,000 |
| 每个 IP 配置和每个 NIC 的应用程序安全组数 |20 |
| 每个应用程序安全组的 IP 配置数 |4,000 |
| 可在网络安全组的所有安全规则中指定的应用程序安全组数 |100 |
| 用户定义的路由表 |200 |
| 用户定义的路由每个路由表 |400 |
| 每个 Azure VPN 网关的点到站点的根证书 |20 |
| 虚拟网络 TAP |100 |
| 每个虚拟网络 TAP 的网络接口 TAP 配置 |100 |

#### <a name="publicip-address"></a>公共 IP 地址限制
| 资源 | 默认限制 | 最大限制 |
| --- | --- | --- |
| 公共 IP 地址数 - 动态 | Basic 乘以 1000。 |联系支持人员。 |
| 公共 IP 地址数 - 静态 | basic 200。 |联系支持人员。 |
| 公共 IP 地址数 - 静态 | 标准 200。|联系支持人员。 |
| 公共 IP 前缀大小 （预览版） | /28 | /28 |

#### <a name="load-balancer"></a>负载均衡器限制
以下限制仅适用于每个订阅按区域通过 Azure 资源管理器管理的网络资源。 了解如何[针对订阅限制查看当前资源使用情况](../articles/networking/check-usage-against-limits.md)。

| 资源 | 默认限制 |
| --- | --- |
| 负载均衡器 | 1,000 | 
| 每个资源的规则数，基本 | 250 |
| 每个资源的规则数，标准 | 1,500 | 
| 每个 IP 配置的规则数 | 299 |
| 每个 NIC 的规则数 | 500 |
| 前端 IP 配置数基本 | 200 |
| 前端 IP 配置数标准 | 600 |
| 后端池基本 | 100，单个可用性集中 |
| 后端池标准 | 1000，单个虚拟网络 |
| 每个负载均衡器，标准的后端资源<sup>1</sup> | 150 |
| 高可用性端口标准 | 每 1 个内部前端 |

<sup>1</sup>上限为最多 150 个资源，在独立虚拟机资源的任意组合，可用性集资源和虚拟机规模集资源。

