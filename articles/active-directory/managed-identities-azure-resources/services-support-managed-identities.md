---
title: 支持 Azure 资源托管标识的 Azure 服务
description: 支持 Azure 资源托管标识和 Azure AD 身份验证的服务列表
services: active-directory
author: priyamohanram
ms.author: priyamo
ms.date: 11/28/2018
ms.topic: conceptual
ms.service: active-directory
ms.subservice: msi
manager: daveba
ms.collection: M365-identity-device-management
ms.openlocfilehash: 1f6c511c605a98a696fb542638e86e63f0226150
ms.sourcegitcommit: 02d17ef9aff49423bef5b322a9315f7eab86d8ff
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/21/2019
ms.locfileid: "58340250"
---
# <a name="services-that-support-managed-identities-for-azure-resources"></a>支持 Azure 资源托管标识的服务

Azure 资源的托管标识在 Azure Active Directory 中为 Azure 服务提供了一个自动托管标识。 使用托管标识，可对支持 Azure AD 身份验证的任何服务进行身份验证，而无需在代码中插入凭据。 我们正在跨 Azure 将 Azure 资源托管标识与 Azure AD 身份验证集成。 请经常再回来看看，确定这部分内容是否有更新。

> [!NOTE]
> Azure 资源托管标识是以前称为托管服务标识 (MSI) 的服务的新名称。

## <a name="azure-services-that-support-managed-identities-for-azure-resources"></a>支持 Azure 资源托管标识的 Azure 服务

以下 Azure 服务支持 Azure 资源托管标识：

### <a name="azure-virtual-machines"></a>Azure 虚拟机

| 托管标识类型 | 所有正式发布版<br>全球 Azure 区域 | Azure Government  | Azure 德国 | Azure 中国世纪互联 |
| --- | --- | --- | --- | --- |
| 系统分配 | 可用 | 预览 | 预览 | 预览 | 
| 用户分配 | 预览 | 预览 | 预览 | 预览 |

请参阅以下列表来配置 Azure 虚拟机的托管标识（在可用的区域中）：

- [Azure 门户](qs-configure-portal-windows-vm.md)
- [PowerShell](qs-configure-powershell-windows-vm.md)
- [Azure CLI](qs-configure-cli-windows-vm.md)
- [Azure 资源管理器模板](qs-configure-template-windows-vm.md)
- [REST](qs-configure-rest-vm.md)

### <a name="azure-virtual-machine-scale-sets"></a>Azure 虚拟机规模集

|托管标识类型 | 所有正式发布版<br>全球 Azure 区域 | Azure Government  | Azure 德国 | Azure 中国世纪互联 |
| --- | --- | --- | --- | --- |
| 系统分配 | 可用 | 预览 | 预览 | 预览 |
| 用户分配 | 预览 | 预览 | 预览 | 预览 |

请参阅以下列表来配置 Azure 虚拟机规模集的托管标识（在可用的区域中）：

- [Azure 门户](qs-configure-portal-windows-vm.md)
- [PowerShell](qs-configure-powershell-windows-vm.md)
- [Azure CLI](qs-configure-cli-windows-vm.md)
- [Azure 资源管理器模板](qs-configure-template-windows-vm.md)
- [REST](qs-configure-rest-vm.md)

### <a name="azure-app-service"></a>Azure 应用服务

| 托管标识类型 | 所有正式发布版<br>全球 Azure 区域 | Azure Government  | Azure 德国 | Azure 中国世纪互联 |
| --- | --- | --- | --- | --- |
| 系统分配 | 可用 | 可用 | 可用 | 可用 |
| 用户分配 | 预览 | 不可用 | 不可用 | 不可用 |

请参阅以下列表来配置 Azure 应用服务的托管标识（在可用的区域中）：

- [Azure 门户](/azure/app-service/overview-managed-identity#using-the-azure-portal)
- [Azure CLI](/azure/app-service/overview-managed-identity#using-the-azure-cli)
- [Azure PowerShell](/azure/app-service/overview-managed-identity#using-azure-powershell)
- [Azure 资源管理器模板](/azure/app-service/overview-managed-identity#using-an-azure-resource-manager-template)

### <a name="azure-blueprints"></a>Azure 蓝图

|托管标识类型 | 所有正式发布版<br>全球 Azure 区域 | Azure Government  | Azure 德国 | Azure 中国世纪互联 |
| --- | --- | --- | --- | --- |
| 系统分配 | 预览 | 不可用 | 不可用 | 不可用 |
| 用户分配 | 预览 | 不可用 | 不可用 | 不可用 |

请参阅以下列表来使用与托管的标识[Azure 蓝图](../../governance/blueprints/overview.md):

- [Azure 门户-蓝图分配](../../governance/blueprints/create-blueprint-portal.md#assign-a-blueprint)
- [REST API-蓝图分配](../../governance/blueprints/create-blueprint-rest-api.md#assign-a-blueprint)

### <a name="azure-functions"></a>Azure Functions

托管标识类型 |所有正式发布版<br>全球 Azure 区域 | Azure Government  | Azure 德国 | Azure 中国世纪互联 |
| --- | --- | --- | --- | --- |
| 系统分配 | 可用 | 可用 | 可用 | 可用 |
| 用户分配 | 预览 | 不可用 | 不可用 | 不可用 |

请参阅以下列表来配置 Azure Functions 的托管标识（在可用的区域中）：

- [Azure 门户](/azure/app-service/overview-managed-identity#using-the-azure-portal)
- [Azure CLI](/azure/app-service/overview-managed-identity#using-the-azure-cli)
- [Azure PowerShell](/azure/app-service/overview-managed-identity#using-azure-powershell)
- [Azure 资源管理器模板](/azure/app-service/overview-managed-identity#using-an-azure-resource-manager-template)

### <a name="azure-logic-apps"></a>Azure 逻辑应用

托管标识类型 | 所有正式发布版<br>全球 Azure 区域 | Azure Government  | Azure 德国 | Azure 中国世纪互联 |
| --- | --- | --- | --- | --- |
| 系统分配 | 预览 | 预览 | 不可用 | 预览 |
| 用户分配 | 不可用 | 不可用 | 不可用 | 不可用 |

请参阅以下列表来配置 Azure 逻辑应用的托管标识（在可用的区域中）：

- [Azure 门户](/azure/logic-apps/create-managed-service-identity#azure-portal)
- [Azure 资源管理器模板](/azure/app-service/overview-managed-identity)

### <a name="azure-data-factory-v2"></a>Azure 数据工厂 V2

托管标识类型 | 所有正式发布版<br>全球 Azure 区域 | Azure Government  | Azure 德国 | Azure 中国世纪互联 |
| --- | --- | --- | --- | --- |
| 系统分配 | 可用 | 不可用 | 不可用 | 不可用 |
| 用户分配 | 不可用 | 不可用 | 不可用 | 不可用 |

请参阅以下列表来配置 Azure 数据工厂 V2 的托管标识（在可用的区域中）：

- [Azure 门户](~/articles/data-factory/data-factory-service-identity.md#generate-managed-identity)
- [PowerShell](~/articles/data-factory/data-factory-service-identity.md#generate-managed-identity-using-powershell)
- [REST](~/articles/data-factory/data-factory-service-identity.md#generate-managed-identity-using-rest-api)
- [SDK](~/articles/data-factory/data-factory-service-identity.md#generate-managed-identity-using-sdk)

### <a name="azure-api-management"></a>Azure API 管理

托管标识类型 | 所有正式发布版<br>全球 Azure 区域 | Azure Government  | Azure 德国 | Azure 中国世纪互联 |
| --- | --- | --- | --- | --- |
| 系统分配 | 可用 | 可用 | 不可用 | 不可用 |
| 用户分配 | 不可用 | 不可用 | 不可用 | 不可用 |

请参阅以下列表来配置 Azure API 管理的托管标识（在可用的区域中）：

- [Azure 资源管理器模板](/azure/api-management/api-management-howto-use-managed-service-identity)

### <a name="azure-container-instances"></a>Azure 容器实例

托管标识类型 | 所有正式发布版<br>全球 Azure 区域 | Azure Government  | Azure 德国 | Azure 中国世纪互联 |
| --- | --- | --- | --- | --- |
| 系统分配 | Linux：预览<br>Windows:不可用 | 不可用 | 不可用 | 不可用 |
| 用户分配 | Linux：预览<br>Windows:不可用 | 不可用 | 不可用 | 不可用 |

请参阅以下列表来配置 Azure 容器实例的托管标识（在可用的区域中）：

- [Azure CLI](~/articles/container-instances/container-instances-managed-identity.md)
- [Azure 资源管理器模板](~/articles/container-instances/container-instances-managed-identity.md#enable-managed-identity-using-resource-manager-template)
- [YAML](~/articles/container-instances/container-instances-managed-identity.md#enable-managed-identity-using-yaml-file)

## <a name="azure-services-that-support-azure-ad-authentication"></a>支持 Azure AD 身份验证的 Azure 服务

以下服务支持 Azure AD 身份验证，已通过使用 Azure 资源托管标识的客户端服务进行测试。

### <a name="azure-resource-manager"></a>Azure 资源管理器

请参阅以下列表来配置到 Azure 资源管理器的访问权限：

- [将通过 Azure 门户的访问权限分配](howto-assign-access-portal.md)
- [将通过 Powershell 的访问权限分配](howto-assign-access-powershell.md)
- [分配的访问权限通过 Azure CLI](howto-assign-access-CLI.md)
- [分配的访问权限通过 Azure 资源管理器模板](../../role-based-access-control/role-assignments-template.md)

| 云 | 资源 ID | 状态 |
|--------|------------|--------|
| Azure 全球 | `https://management.azure.com/`| 可用 |
| Azure Government  | `https://management.usgovcloudapi.net/` | 可用 |
| Azure 德国 | `https://management.microsoftazure.de/` | 可用 |
| Azure 中国世纪互联 | `https://management.chinacloudapi.cn` | 可用 |

### <a name="azure-key-vault"></a>Azure 密钥保管库

| 云 | 资源 ID | 状态 |
|--------|------------|--------|
| Azure 全球 | `https://vault.azure.net`| 可用 |
| Azure Government  | `https://vault.usgovcloudapi.net` | 可用 |
| Azure 德国 |  `https://vault.microsoftazure.de` | 可用 |
| Azure 中国世纪互联 | `https://vault.azure.cn` | 可用 |

## <a name="azure-data-lake"></a>Azure Data Lake 

| 云 | 资源 ID | 状态 |
|--------|------------|--------|
| Azure 全球 | `https://datalake.azure.net/` | 可用 |
| Azure Government  |  | 不可用 |
| Azure 德国 |   | 不可用 |
| Azure 中国世纪互联 |  | 不可用 |

## <a name="azure-sql"></a>Azure SQL 

| 云 | 资源 ID | 状态 |
|--------|------------|--------|
| Azure 全球 | `https://database.windows.net/` | 可用 |
| Azure Government  | `https://database.usgovcloudapi.net/` | 可用 |
| Azure 德国 | `https://database.cloudapi.de/` | 可用 |
| Azure 中国世纪互联 | `https://database.chinacloudapi.cn/` | 可用 |

## <a name="azure-event-hubs"></a>Azure 事件中心

| 云 | 资源 ID | 状态 |
|--------|------------|--------|
| Azure 全球 | `https://eventhubs.azure.net` | 预览 |
| Azure Government  |  | 不可用 |
| Azure 德国 |   | 不可用 |
| Azure 中国世纪互联 |  | 不可用 |

## <a name="azure-service-bus"></a>Azure 服务总线

| 云 | 资源 ID | 状态 |
|--------|------------|--------|
| Azure 全球 | `https://servicebus.azure.net`  | 预览 |
| Azure Government  |  | 不可用 |
| Azure 德国 |   | 不可用 |
| Azure 中国世纪互联 |  | 不可用 |

## <a name="azure-storage"></a>Azure 存储

| 云 | 资源 ID | 状态 |
|--------|------------|--------|
| Azure 全球 | `https://storage.azure.com/` | 预览 |
| Azure Government  |  | 不可用 |
| Azure 德国 |   | 不可用 |
| Azure 中国世纪互联 |  | 不可用 |
