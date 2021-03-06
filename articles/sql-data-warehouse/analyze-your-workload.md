---
title: 分析工作负荷 - Azure SQL 数据仓库 | Microsoft Docs
description: 分析针对 Azure SQL 数据仓库中工作负荷的查询优化的技巧。
services: sql-data-warehouse
author: ronortloff
manager: craigg
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.subservice: workload management
ms.date: 03/13/2019
ms.author: rortloff
ms.reviewer: jrasnick
ms.openlocfilehash: 434cbb18a109308844dbc7ff219d40948678e86e
ms.sourcegitcommit: 90dcc3d427af1264d6ac2b9bde6cdad364ceefcc
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/21/2019
ms.locfileid: "58310721"
---
# <a name="analyze-your-workload-in-azure-sql-data-warehouse"></a>分析 Azure SQL 数据仓库中的工作负荷

用于分析 Azure SQL 数据仓库中的工作负荷的技术。

## <a name="resource-classes"></a>资源类

SQL 数据仓库提供系统资源分配给查询的资源类。  资源类的详细信息，请参阅[资源类和工作负荷管理](resource-classes-for-workload-management.md)。  如果分配给查询的资源类需要更多的资源比当前可用的查询将一直等待。

## <a name="queued-query-detection-and-other-dmvs"></a>对排队的查询进行的检测，以及其他 DMV

可以使用 `sys.dm_pdw_exec_requests` DMV 来确定在并发队列中等待的查询。 正在等待并发槽的查询的状态为“已挂起”。

```sql
SELECT  r.[request_id]                           AS Request_ID
,       r.[status]                               AS Request_Status
,       r.[submit_time]                          AS Request_SubmitTime
,       r.[start_time]                           AS Request_StartTime
,       DATEDIFF(ms,[submit_time],[start_time])  AS Request_InitiateDuration_ms
,       r.resource_class                         AS Request_resource_class
FROM    sys.dm_pdw_exec_requests r
;
```

可以使用 `sys.database_principals` 来查看工作负荷管理角色。

```sql
SELECT  ro.[name]           AS [db_role_name]
FROM    sys.database_principals ro
WHERE   ro.[type_desc]      = 'DATABASE_ROLE'
AND     ro.[is_fixed_role]  = 0
;
```

以下查询显示分配给每个用户的角色。

```sql
SELECT  r.name AS role_principal_name
,       m.name AS member_principal_name
FROM    sys.database_role_members rm
JOIN    sys.database_principals AS r            ON rm.role_principal_id      = r.principal_id
JOIN    sys.database_principals AS m            ON rm.member_principal_id    = m.principal_id
WHERE   r.name IN ('mediumrc','largerc','xlargerc')
;
```

SQL 数据仓库具有以下等待类型：

* **LocalQueriesConcurrencyResourceType**：位于并发槽框架外部的查询。 DMV 查询和 `SELECT @@VERSION` 等系统函数是本地查询的示例。
* **UserConcurrencyResourceType**：位于并发槽框架内部的查询。 针对最终用户表的查询代表使用此资源类型的示例。
* **DmsConcurrencyResourceType**：数据移动操作导致的等待。
* **BackupConcurrencyResourceType**：此等待表明正在备份数据库。 此资源类型的最大值为 1。 如果同时请求了多个备份，则其他备份会排队。 一般情况下，我们建议为 10 分钟的连续快照之间最短时间。 

可以使用 `sys.dm_pdw_waits` DMV 来查看请求所等待的具体资源。

```sql
SELECT  w.[wait_id]
,       w.[session_id]
,       w.[type]                                           AS Wait_type
,       w.[object_type]
,       w.[object_name]
,       w.[request_id]
,       w.[request_time]
,       w.[acquire_time]
,       w.[state]
,       w.[priority]
,       SESSION_ID()                                       AS Current_session
,       s.[status]                                         AS Session_status
,       s.[login_name]
,       s.[query_count]
,       s.[client_id]
,       s.[sql_spid]
,       r.[command]                                        AS Request_command
,       r.[label]
,       r.[status]                                         AS Request_status
,       r.[submit_time]
,       r.[start_time]
,       r.[end_compile_time]
,       r.[end_time]
,       DATEDIFF(ms,r.[submit_time],r.[start_time])        AS Request_queue_time_ms
,       DATEDIFF(ms,r.[start_time],r.[end_compile_time])   AS Request_compile_time_ms
,       DATEDIFF(ms,r.[end_compile_time],r.[end_time])     AS Request_execution_time_ms
,       r.[total_elapsed_time]
FROM    sys.dm_pdw_waits w
JOIN    sys.dm_pdw_exec_sessions s  ON w.[session_id] = s.[session_id]
JOIN    sys.dm_pdw_exec_requests r  ON w.[request_id] = r.[request_id]
WHERE    w.[session_id] <> SESSION_ID()
;
```

`sys.dm_pdw_resource_waits` DMV 显示给定查询的等待信息。 资源等待时间度量等待提供资源的时间。 信号等待时间为基础的 SQL server，若要将查询调度到 CPU 所需的时间。

```sql
SELECT  [session_id]
,       [type]
,       [object_type]
,       [object_name]
,       [request_id]
,       [request_time]
,       [acquire_time]
,       DATEDIFF(ms,[request_time],[acquire_time])  AS acquire_duration_ms
,       [concurrency_slots_used]                    AS concurrency_slots_reserved
,       [resource_class]
,       [wait_id]                                   AS queue_position
FROM    sys.dm_pdw_resource_waits
WHERE    [session_id] <> SESSION_ID()
;
```

还可以使用 `sys.dm_pdw_resource_waits` DMV 计算已授予的并发槽数。

```sql
SELECT  SUM([concurrency_slots_used]) as total_granted_slots
FROM    sys.[dm_pdw_resource_waits]
WHERE   [state]           = 'Granted'
AND     [resource_class] is not null
AND     [session_id]     <> session_id()
;
```

可以使用 `sys.dm_pdw_wait_stats` DMV 对等待进行历史趋势分析。

```sql
SELECT   w.[pdw_node_id]
,        w.[wait_name]
,        w.[max_wait_time]
,        w.[request_count]
,        w.[signal_time]
,        w.[completed_count]
,        w.[wait_time]
FROM    sys.dm_pdw_wait_stats w
;
```

## <a name="next-steps"></a>后续步骤

有关如何管理数据库用户和安全性的详细信息，请参阅[保护 SQL 数据仓库中的数据库](sql-data-warehouse-overview-manage-security.md)。 有关如何通过更大型资源类来改进聚集列存储索引质量的详细信息，请参阅[重新生成索引以提升段质量](sql-data-warehouse-tables-index.md#rebuilding-indexes-to-improve-segment-quality)。
