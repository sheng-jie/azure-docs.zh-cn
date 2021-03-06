---
author: roygara
ms.service: virtual-machines
ms.topic: include
ms.date: 03/18/2019
ms.author: rogarana
ms.openlocfilehash: 2936fd318f08c74675f7e8b382c861f4a28319fc
ms.sourcegitcommit: 12d67f9e4956bb30e7ca55209dd15d51a692d4f6
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/20/2019
ms.locfileid: "58261360"
---
可以将多个数据磁盘附加到 Azure 虚拟机。 根据 VM 的数据磁盘的可伸缩性和性能目标，可以确定的数量和类型满足性能和容量要求所需的磁盘。

> [!IMPORTANT]
> 为了获得最佳性能，需要限制附加到虚拟机的、重度使用的磁盘数，以避免可能的性能限制。 如果所有附加的磁盘不高利用率在同一时间，虚拟机可以支持更多的磁盘。

**Azure 托管磁盘：**

下表说明了默认值和每个区域每个订阅的资源数的最大限制

> | 资源 | 默认限制  | 最大限制 |
> | --- | --- | --- |
> | 标准托管磁盘 | 25,000 | 50,000 |
> | 标准 SSD 托管磁盘 | 25,000 | 50,000 |
> | 高级托管磁盘 | 25,000 | 50,000 |
> | Standard_LRS 快照 | 25,000 | 50,000 |
> | Standard_ZRS 快照 | 25,000 | 50,000 |
> | 托管的映像 | 25,000 | 50,000 |

* **对于标准存储帐户：** 标准存储帐户的总请求率上限为 20,000 IOPS。 在所有标准存储帐户中虚拟机磁盘的 IOPS 总数不应超过此限制。
  
    大致可以计算支持的基于请求速率限制单个标准存储帐户的利用率较高的磁盘数。 例如，对于基本层 VM，重度使用磁盘的最大数目是大约 66，即每个磁盘 20000/300 IOPS。 标准层 VM 的磁盘利用率较高的最大数目是约为 40，即每个磁盘 20000/500 IOPS。 

* **高级存储帐户：** 高级存储帐户的总吞吐量速率上限为 50 Gbps。 所有 VM 磁盘的总吞吐量不应超过此限制。

