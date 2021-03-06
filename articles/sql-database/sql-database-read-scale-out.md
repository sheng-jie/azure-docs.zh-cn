---
title: Azure SQL 数据库 - 读取副本中的查询 | Microsoft Docs
description: Azure SQL 数据库允许使用只读副本的容量（称为“读取横向扩展”）对只读的工作负荷进行负载均衡。
services: sql-database
ms.service: sql-database
ms.subservice: scale-out
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: anosov1960
ms.author: sashan
ms.reviewer: sstein, carlrab
manager: craigg
ms.date: 03/12/2019
ms.openlocfilehash: 8f34b3ed91e4b470fdfa7c2ffad401e7890abe1e
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/19/2019
ms.locfileid: "57886450"
---
# <a name="use-read-only-replicas-to-load-balance-read-only-query-workloads-preview"></a>使用只读副本对只读的查询工作负荷进行负载均衡（预览版）

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]
> [!IMPORTANT]
> PowerShell Azure 资源管理器模块仍受 Azure SQL 数据库，但未来的所有开发都不适用于 Az.Sql 模块。 有关这些 cmdlet，请参阅[AzureRM.Sql](https://docs.microsoft.com/powershell/module/AzureRM.Sql/)。 命令在 Az 模块和 AzureRm 模块中的参数是大体上相同的。

“读取扩展”允许使用一个只读副本的容量对 Azure SQL 数据库只读工作负荷进行负载均衡。

“高级”层（[基于 DTU 的购买模型](sql-database-service-tiers-dtu.md)）或“业务关键”层（[基于 vCore 的购买模型](sql-database-service-tiers-vcore.md)）中的每个数据库中已自动预配多个 Always ON 副本，以支持可用性 SLA。

![只读副本](media/sql-database-managed-instance/business-critical-service-tier.png)

为这些副本预配的计算大小与常规数据库连接使用的读写副本相同。 “读取扩展”功能允许使用一个只读副本的容量而不是共享读写副本，对 SQL 数据库只读工作负载进行负载均衡。 这样，只读工作负荷将与主要的读写工作负荷相隔离，不会影响其性能。 该功能适用于其中包括逻辑上独立的只读工作负荷（例如分析）的应用程序，因此可以在不增加成本的情况下使用此额外容量来获得性能优势。

若要使用特定数据库的读取横向扩展功能，您必须显式启用它创建数据库时或之后通过更改其配置使用 PowerShell 通过调用[集 AzSqlDatabase](/powershell/module/az.sql/set-azsqldatabase)或[新建 AzSqlDatabase](/powershell/module/az.sql/new-azsqldatabase) cmdlet 或通过 Azure 资源管理器 REST API 使用[数据库-创建或更新](https://docs.microsoft.com/rest/api/sql/databases/createorupdate)方法。

为某个数据库启用读取横向扩展后，会根据在应用程序的连接字符串中配置的 `ApplicationIntent` 属性将连接到该数据库的应用程序定向到该数据库的读写副本或只读副本。 有关 `ApplicationIntent` 属性的信息，请参阅[指定应用程序意向](https://docs.microsoft.com/sql/relational-databases/native-client/features/sql-server-native-client-support-for-high-availability-disaster-recovery#specifying-application-intent)。

如果禁用了“读取扩展”，或在不支持的服务层中设置了 ReadScale 属性，则所有连接都将定向到读写副本，而与 `ApplicationIntent` 属性无关。

> [!NOTE]
> 副本的只读副本上不支持查询数据存储和扩展事件。

## <a name="data-consistency"></a>数据一致性

副本的优势之一是，它始终处于事务一致性状态，但在不同的时间点，不同的副本之间可能会有一些较小的延迟。 读取横向扩展支持会话级一致性。 这意味着，如果只读会话在由于副本不可用而出现连接错误后重新连接，可以使用读写副本将其重定向到并非完全处于最新状态的副本。 同样，如果应用程序使用读写会话写入数据，并立即使用只读会话读取该数据，则最新的更新可能不会立即可见。 这是因为，对副本执行的事务日志重做是异步进行的。

> [!NOTE]
> 区域中的复制延迟较低，且这种情况很少见。

## <a name="connect-to-a-read-only-replica"></a>连接到只读副本

为数据库启用读取横向扩展时，客户端提供的连接字符串中的 `ApplicationIntent` 选项会指示连接是要路由到写入副本还是只读副本。 具体而言，如果 `ApplicationIntent` 值为 `ReadWrite`（默认值），则连接将定向到数据库的读写副本。 这与现有行为相同。 如果 `ApplicationIntent` 值为 `ReadOnly`，则连接将路由到只读副本。

例如，以下连接字符串将客户端连接到只读副本（请将尖括号中的项替换为环境的正确值，并删除尖括号）：

```SQL
Server=tcp:<server>.database.windows.net;Database=<mydatabase>;ApplicationIntent=ReadOnly;User ID=<myLogin>;Password=<myPassword>;Trusted_Connection=False; Encrypt=True;
```

以下连接字符串之一将客户端连接到读写副本（请将尖括号中的项替换为环境的正确值，并删除尖括号）：

```SQL
Server=tcp:<server>.database.windows.net;Database=<mydatabase>;ApplicationIntent=ReadWrite;User ID=<myLogin>;Password=<myPassword>;Trusted_Connection=False; Encrypt=True;

Server=tcp:<server>.database.windows.net;Database=<mydatabase>;User ID=<myLogin>;Password=<myPassword>;Trusted_Connection=False; Encrypt=True;
```

## <a name="verify-that-a-connection-is-to-a-read-only-replica"></a>验证连接到只读副本

可通过运行以下查询来验证是否连接到只读副本。 连接到只读副本时，它将返回 READ_ONLY。

```SQL
SELECT DATABASEPROPERTYEX(DB_NAME(), 'Updateability')
```

> [!NOTE]
> 在任何给定时间，ReadOnly 会话只能访问一个 AlwaysON 副本。

## <a name="enable-and-disable-read-scale-out"></a>启用和禁用读取扩展

默认情况下，[托管实例](sql-database-managed-instance.md)业务关键层中启用了读取扩展。 它在 [SQL 数据库服务器上放置的数据库](sql-database-servers.md)高级和业务关键层中应处于显式启用状态。 下面介绍了启用和禁用读取扩展的方法。

### <a name="powershell-enable-and-disable-read-scale-out"></a>PowerShell：启用和禁用读取扩展

在 Azure PowerShell 中管理读取横向扩展需要安装 Azure PowerShell 2016 年 12 月版或更高版本。 有关最新的 PowerShell 版本，请参阅 [Azure PowerShell](https://docs.microsoft.com/powershell/azure/install-az-ps)。

启用或禁用读取的横向扩展在 Azure PowerShell 中通过调用[集 AzSqlDatabase](/powershell/module/az.sql/set-azsqldatabase) cmdlet 并传入所需的值 –`Enabled`或`Disabled`-为`-ReadScale`参数。 或者，可以使用[新建 AzSqlDatabase](/powershell/module/az.sql/new-azsqldatabase) cmdlet 创建一个新数据库读取横向扩展已启用。

例如，若要为现有数据库启用读取横向扩展（请将尖括号中的项替换为环境的正确值，并删除尖括号）：

```powershell
Set-AzSqlDatabase -ResourceGroupName <myresourcegroup> -ServerName <myserver> -DatabaseName <mydatabase> -ReadScale Enabled
```

若要为现有数据库禁用读取横向扩展（请将尖括号中的项替换为环境的正确值，并删除尖括号）：

```powershell
Set-AzSqlDatabase -ResourceGroupName <myresourcegroup> -ServerName <myserver> -DatabaseName <mydatabase> -ReadScale Disabled
```

若要创建已启用读取横向扩展的新数据库（请将尖括号中的项替换为环境的正确值，并删除尖括号）：

```powershell
New-AzSqlDatabase -ResourceGroupName <myresourcegroup> -ServerName <myserver> -DatabaseName <mydatabase> -ReadScale Enabled -Edition Premium
```

### <a name="rest-api-enable-and-disable-read-scale-out"></a>REST API：启用和禁用读取扩展

若要创建已启用读取横向扩展的数据库，或者要为现有数据库启用或禁用读取横向扩展，请按以下示例请求中所示，在 `readScale` 属性设置为 `Enabled` 或 `Disabled` 的情况下，创建或更新相应的数据库实体。

```rest
Method: PUT
URL: https://management.azure.com/subscriptions/{SubscriptionId}/resourceGroups/{GroupName}/providers/Microsoft.Sql/servers/{ServerName}/databases/{DatabaseName}?api-version= 2014-04-01-preview
Body:
{
   "properties":
   {
      "readScale":"Enabled"
   }
}
```

有关详细信息，请参阅[数据库 - 创建或更新](https://docs.microsoft.com/rest/api/sql/databases/createorupdate)。

## <a name="using-read-scale-out-with-geo-replicated-databases"></a>结合使用读取扩展与异地复制的数据库

如果正在使用读取扩展在异地复制数据库（例如，作为故障转移组的一个成员）上对只读工作负载进行负载均衡，请确保主数据库和异地复制辅助数据库上都启用了读取扩展。 这可确保应用程序在故障转移后连接到新的主数据库时，具有相同的负载均衡效果。 如果要连接到启用了读取扩展的异地复制辅助数据库，则设置为 `ApplicationIntent=ReadOnly` 的会话将路由到其中一个副本，就像我们在主数据库上路由连接一样。  而未设为 `ApplicationIntent=ReadOnly` 的会话将路由到异地复制辅助数据库的主要副本，该副本也为只读。 由于异地复制辅助数据库的终结点与主数据库不同，因此之前访问辅助数据库不需要设置 `ApplicationIntent=ReadOnly`。 为确保后向兼容性，`sys.geo_replication_links` DMV 显示`secondary_allow_connections=2`（允许的任何客户端连接）。

> [!NOTE]
> 不支持轮循机制或任何其他负载平衡辅助数据库的本地副本之间的路由。

## <a name="next-steps"></a>后续步骤

- 有关使用 PowerShell 设置读取的横向扩展的信息，请参阅[集 AzSqlDatabase](/powershell/module/az.sql/set-azsqldatabase)或[新建 AzSqlDatabase](/powershell/module/az.sql/new-azsqldatabase) cmdlet。
- 有关使用 REST API 设置读取横向扩展的信息，请参阅[数据库 - 创建或更新](https://docs.microsoft.com/rest/api/sql/databases/createorupdate)。
