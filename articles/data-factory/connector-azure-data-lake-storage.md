---
title: 使用数据工厂向/从 Azure Data Lake Storage Gen2 复制数据 | Microsoft Docs
description: 了解如何使用 Azure 数据工厂向/从 Azure Data Lake Storage Gen2 复制数据。
services: data-factory
author: linda33wj
manager: craigg
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.date: 02/25/2019
ms.author: jingwang
ms.openlocfilehash: f27e7eba11dd98bc30f4f1b5d796488d3973f64a
ms.sourcegitcommit: 94305d8ee91f217ec98039fde2ac4326761fea22
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/05/2019
ms.locfileid: "57405617"
---
# <a name="copy-data-to-or-from-azure-data-lake-storage-gen2-using-azure-data-factory"></a>使用 Azure 数据工厂向/从 Azure Data Lake Storage Gen2 复制数据

Azure Data Lake Storage Gen2 是一组专用于大数据分析的功能，内置于 [Azure Blob 存储](../storage/blobs/storage-blobs-introduction.md)中。 它可使用文件系统和对象存储范例与数据进行交互。

本文概述了如何使用 Azure 数据工厂中的复制活动向/从 Data Lake Storage Gen2 复制数据。 本文是根据总体概述复制活动的[复制活动概述](copy-activity-overview.md)一文编写的。

## <a name="supported-capabilities"></a>支持的功能

可将数据从任一支持的源数据存储复制到 Data Lake Storage Gen2。 也可将数据从 Data Lake Storage Gen2 复制到任一支持的接收器数据存储。 有关复制活动支持作为源或接收器的数据存储列表，请参阅[支持的数据存储](copy-activity-overview.md)表。

具体而言，此连接器支持：

- 通过使用帐户密钥、服务主体或托管标识进行 Azure 资源身份验证来复制数据。
- 按原样复制文件，或使用[支持的文件格式和压缩编解码器](supported-file-formats-and-compression-codecs.md)分析/生成文件。

>[!TIP]
>如果启用分层命名空间，则当前 Blob 和 ADLS Gen2 API 之间没有操作的互操作性。 如果遇到“ErrorCode=FilesystemNotFound”错误，详细消息为“指定的文件系统不存在。”，这是由于指定的接收器文件系统是通过 Blob API 而非其他地方的 ADLS Gen2 API 创建的。 若要解决此问题，请指定其名称不同于已存在的 Blob 容器名称的一个新文件系统，ADF 在复制数据期间将自动创建该文件系统。

>[!NOTE]
>如果在 Azure 存储防火墙设置上启用“允许受信任的 Microsoft 服务访问此存储帐户”选项，则使用 Azure Integration Runtime 连接 Data Lake Storage Gen2 将失败并显示“已禁止”错误，因为 ADF 不被视为受信任的 Microsoft 服务。 请改用自承载集成运行时进行连接。

## <a name="get-started"></a>入门

>[!TIP]
>有关使用 Data Lake Storage Gen2 连接器的演练，请参阅[将数据加载到 Azure Data Lake Storage Gen2](load-azure-data-lake-storage-gen2.md)。

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

以下部分详细介绍了用于定义 Data Lake Storage Gen2 特定的数据工厂实体的属性。

## <a name="linked-service-properties"></a>链接服务属性

Azure Data Lake Storage Gen2 连接器支持以下身份验证类型，有关详细信息，请参阅相应的部分：

- [帐户密钥身份验证](#account-key-authentication)
- [服务主体身份验证](#service-principal-authentication)
- [Azure 资源的托管标识身份验证](#managed-identity)

### <a name="account-key-authentication"></a>帐户密钥身份验证

若要使用存储帐户密钥身份验证，需支持以下属性：

| 属性 | 说明 | 必选 |
|:--- |:--- |:--- |
| type | type 属性必须设置为 AzureBlobFS。 |是 |
| url | Data Lake Storage Gen2 的终结点，其模式为 `https://<accountname>.dfs.core.windows.net`。 | 是 | 
| accountKey | Data Lake Storage Gen2 服务的帐户密钥。 将此字段标记为 SecureString 以安全地将其存储在数据工厂中或[引用存储在 Azure Key Vault 中的机密](store-credentials-in-key-vault.md)。 |是 |
| connectVia | 用于连接到数据存储的[集成运行时](concepts-integration-runtime.md)。 如果数据存储位于专用网络，则可以使用 Azure 集成运行时或自承载集成运行时。 如果未指定，则使用默认 Azure Integration Runtime。 |否 |

**示例：**

```json
{
    "name": "AzureDataLakeStorageGen2LinkedService",
    "properties": {
        "type": "AzureBlobFS",
        "typeProperties": {
            "url": "https://<accountname>.dfs.core.windows.net", 
            "accountkey": { 
                "type": "SecureString", 
                "value": "<accountkey>" 
            }
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

### <a name="service-principal-authentication"></a>服务主体身份验证

若要使用服务主体身份验证，请执行以下步骤：

1. 遵循[将应用程序注册到 Azure AD 租户](../storage/common/storage-auth-aad-app.md#register-your-application-with-an-azure-ad-tenant)，在 Azure Active Directory (Azure AD) 中注册一个应用程序实体。 记下下面的值，这些值用于定义链接服务：

    - 应用程序 ID
    - 应用程序密钥
    - 租户 ID

2. 授予服务主体适当的权限。

    - **作为源**，在存储资源管理器，请至少授予**读取 + 执行**列表，并复制文件夹和子文件夹中的文件或授予的权限**读取**复制单个文件的权限。 或者，在访问控制 (IAM)，请至少授予**存储 Blob 数据读取器**角色。
    - **作为接收器**，在存储资源管理器，请至少授予**写入 + 执行**文件夹中创建子项目的权限。 或者，在访问控制 (IAM)，请至少授予**存储 Blob 数据参与者**角色。

>[!NOTE]
>到列表文件夹从根目录开始，您需要设置的权限被授予对服务主体**具有"执行"权限的根级别**对 IAM 或权限。 对于以下情况需要这样做：
>- 使用**复制数据工具**创作复制管道。
>- 在创作期间使用**数据工厂 UI** 测试连接和浏览文件夹。 
>如果需考虑授予根级别的权限，则可以跳过测试连接和输入的路径手动在创作过程。 复制活动仍适用，只要通过复制文件的适当权限授予服务主体。

链接服务支持以下属性：

| 属性 | 说明 | 必选 |
|:--- |:--- |:--- |
| type | type 属性必须设置为 AzureBlobFS。 |是 |
| url | Data Lake Storage Gen2 的终结点，其模式为 `https://<accountname>.dfs.core.windows.net`。 | 是 | 
| servicePrincipalId | 指定应用程序的客户端 ID。 | 是 |
| servicePrincipalKey | 指定应用程序的密钥。 将此字段标记为 **SecureString** 以安全地将其存储在数据工厂中或[引用存储在 Azure Key Vault 中的机密](store-credentials-in-key-vault.md)。 | 是 |
| tenant | 指定应用程序的租户信息（域名或租户 ID）。 将鼠标悬停在 Azure 门户右上角进行检索。 | 是 |
| connectVia | 用于连接到数据存储的[集成运行时](concepts-integration-runtime.md)。 如果数据存储位于专用网络，则可以使用 Azure 集成运行时或自承载集成运行时。 如果未指定，则使用默认 Azure Integration Runtime。 |否 |

**示例：**

```json
{
    "name": "AzureDataLakeStorageGen2LinkedService",
    "properties": {
        "type": "AzureBlobFS",
        "typeProperties": {
            "url": "https://<accountname>.dfs.core.windows.net", 
            "servicePrincipalId": "<service principal id>",
            "servicePrincipalKey": {
                "type": "SecureString",
                "value": "<service principal key>"
            },
            "tenant": "<tenant info, e.g. microsoft.onmicrosoft.com>" 
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

### <a name="managed-identity"></a> Azure 资源的托管标识身份验证

可将数据工厂与代表此特定数据工厂的 [Azure 资源托管标识](data-factory-service-identity.md)相关联。 对于 Blob 存储身份验证类似于使用服务主体，可以直接使用此托管的标识。 此指定工厂可通过此方法访问以及从/向 Blob 存储复制数据。

若要使用 Azure 资源的托管标识身份验证，请执行以下步骤：

1. [检索数据工厂托管标识信息](data-factory-service-identity.md#retrieve-managed-identity)通过复制"服务标识应用程序 ID"与工厂一起生成的值。

2. 授予托管的标识适当的权限。 

    - **作为源**，在存储资源管理器，请至少授予**读取 + 执行**列表，并复制文件夹和子文件夹中的文件或授予的权限**读取**复制单个文件的权限。 或者，在访问控制 (IAM)，请至少授予**存储 Blob 数据读取器**角色。
    - **作为接收器**，在存储资源管理器，请至少授予**写入 + 执行**文件夹中创建子项目的权限。 或者，在访问控制 (IAM)，请至少授予**存储 Blob 数据参与者**角色。

>[!NOTE]
>向列表文件夹从根目录开始，您需要设置的权限被授予对托管标识**拥有"Execute"权限的根级别**或 IAM 权限。 对于以下情况需要这样做：
>- 使用**复制数据工具**创作复制管道。
>- 在创作期间使用**数据工厂 UI** 测试连接和浏览文件夹。 
>如果需考虑授予根级别的权限，则可以跳过测试连接和输入的路径手动在创作过程。 复制活动仍适用，只要具有文件要复制的适当权限授予托管的标识。

链接服务支持以下属性：

| 属性 | 说明 | 必选 |
|:--- |:--- |:--- |
| type | type 属性必须设置为 AzureBlobFS。 |是 |
| url | Data Lake Storage Gen2 的终结点，其模式为 `https://<accountname>.dfs.core.windows.net`。 | 是 | 
| connectVia | 用于连接到数据存储的[集成运行时](concepts-integration-runtime.md)。 如果数据存储位于专用网络，则可以使用 Azure 集成运行时或自承载集成运行时。 如果未指定，则使用默认 Azure Integration Runtime。 |否 |

**示例：**

```json
{
    "name": "AzureDataLakeStorageGen2LinkedService",
    "properties": {
        "type": "AzureBlobFS",
        "typeProperties": {
            "url": "https://<accountname>.dfs.core.windows.net", 
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

## <a name="dataset-properties"></a>数据集属性

有关可用于定义数据集的各部分和属性的完整列表，请参阅[数据集](concepts-datasets-linked-services.md)一文。 Azure Data Lake Storage 数据集支持以下属性：

| 属性 | 说明 | 必选 |
|:--- |:--- |:--- |
| type | 数据集的 type 属性必须设置为 AzureBlobFSFile。 |是 |
| folderPath | Data Lake Storage Gen2 中的文件夹的路径。 如果未指定，它指向根目录。 <br/><br/>支持通配符筛选器，允许的通配符为：`*`（匹配零个或更多个字符）和 `?`（匹配零个或单个字符）；如果实际文件夹名称中包含通配符或 `^`，请使用此转义字符进行转义。 <br/><br/>示例：“rootfolder/subfolder/”，请参阅[文件夹和文件筛选器示例](#folder-and-file-filter-examples)中的更多示例。 |否 |
| fileName | 指定“folderPath”下的文件的“名称或通配符筛选器”。 如果没有为此属性指定任何值，则数据集会指向文件夹中的所有文件。 <br/><br/>对于筛选器，允许的通配符为：`*`（匹配零个或更多字符）和 `?`（匹配零个或单个字符）。<br/>- 示例 1：`"fileName": "*.csv"`<br/>- 示例 2：`"fileName": "???20180427.txt"`<br/>如果实际文件名内具有通配符或此转义符，请使用 `^` 进行转义。<br/><br/>如果没有为输出数据集指定 fileName，并且没有在活动接收器中指定 preserveHierarchy，则复制活动会自动生成采用以下模式的文件名称：“Data.[activity run id GUID].[GUID if FlattenHierarchy].[format if configured].[compression if configured]”，例如“Data.0a405f8a-93ff-4c6f-b3be-f69616f1df7a.txt.gz”；如果使用表名称而不是查询从表格源进行复制，则名称模式为“[table name].[format].[compression if configured]”，例如“MyTable.csv”。 |否 |
| modifiedDatetimeStart | 基于属性“上次修改时间”的文件筛选器。 如果文件的上次修改时间在 `modifiedDatetimeStart` 和 `modifiedDatetimeEnd` 之间的时间范围内，则将选中这些文件。 该时间应用于 UTC 时区，格式为“2018-12-01T05:00:00Z”。 <br/><br/> 属性可以为 NULL，这意味着不向数据集应用任何文件特性筛选器。  如果 `modifiedDatetimeStart` 具有日期/时间值，但 `modifiedDatetimeEnd` 为 NULL，则意味着将选中“上次修改时间”属性大于或等于该日期/时间值的文件。  如果 `modifiedDatetimeEnd` 具有日期/时间值，但 `modifiedDatetimeStart` 为 NULL，则意味着将选中“上次修改时间”属性小于该日期/时间值的文件。| 否 |
| modifiedDatetimeEnd | 基于属性“上次修改时间”的文件筛选器。 如果文件的上次修改时间在 `modifiedDatetimeStart` 和 `modifiedDatetimeEnd` 之间的时间范围内，则将选中这些文件。 该时间应用于 UTC 时区，格式为“2018-12-01T05:00:00Z”。 <br/><br/> 属性可以为 NULL，这意味着不向数据集应用任何文件特性筛选器。  如果 `modifiedDatetimeStart` 具有日期/时间值，但 `modifiedDatetimeEnd` 为 NULL，则意味着将选中“上次修改时间”属性大于或等于该日期/时间值的文件。  如果 `modifiedDatetimeEnd` 具有日期/时间值，但 `modifiedDatetimeStart` 为 NULL，则意味着将选中“上次修改时间”属性小于该日期/时间值的文件。| 否 |
| 格式 | 若要在基于文件的存储之间按原样复制文件（二进制副本），可以在输入和输出数据集定义中跳过格式节。<br/><br/>如果要分析或生成具有特定格式的文件，以下是受支持的文件格式类型：TextFormat、JsonFormat、AvroFormat、OrcFormat 和 ParquetFormat。 请将 **format** 中的 **type** 属性设置为上述值之一。 有关详细信息，请参阅[文本格式](supported-file-formats-and-compression-codecs.md#text-format)、[JSON 格式](supported-file-formats-and-compression-codecs.md#json-format)、[Avro 格式](supported-file-formats-and-compression-codecs.md#avro-format)、[Orc 格式](supported-file-formats-and-compression-codecs.md#orc-format)和 [Parquet 格式](supported-file-formats-and-compression-codecs.md#parquet-format)部分。 |否（仅适用于二进制复制方案） |
| compression | 指定数据的压缩类型和级别。 有关详细信息，请参阅[受支持的文件格式和压缩编解码器](supported-file-formats-and-compression-codecs.md#compression-support)。<br/>支持的类型为 **GZip**、**Deflate**、**BZip2** 和 **ZipDeflate**。<br/>支持的级别为“最佳”和“最快”。 |否 |

>[!TIP]
>如需复制文件夹下的所有文件，请仅指定 **folderPath**。<br>如需复制具有给定名称的单个文件，请指定文件夹部分的 **folderPath** 和文件名部分的 **fileName**。<br>如需复制文件夹下的文件子集，请指定文件夹部分的 **folderPath** 和通配符筛选器部分的 **fileName**。 

**示例：**

```json
{
    "name": "AzureDataLakeStorageDataset",
    "properties": {
        "type": "AzureBlobFSFile",
        "linkedServiceName": {
            "referenceName": "<Azure Data Lake Storage Gen2 linked service name>",
            "type": "LinkedServiceReference"
        },
        "typeProperties": {
            "folderPath": "mycontainer/myfolder",
            "fileName": "*",
            "modifiedDatetimeStart": "2018-12-01T05:00:00Z",
            "modifiedDatetimeEnd": "2018-12-01T06:00:00Z",
            "format": {
                "type": "TextFormat",
                "columnDelimiter": ",",
                "rowDelimiter": "\n"
            },
            "compression": {
                "type": "GZip",
                "level": "Optimal"
            }
        }
    }
}
```

## <a name="copy-activity-properties"></a>复制活动属性

要完整了解可供活动定义使用的各个部分和属性，请参阅[复制活动配置](copy-activity-overview.md#configuration)和[管道和活动](concepts-pipelines-activities.md)文章。 本部分列出了 Data Lake Storage Gen2 源和接收器支持的属性。

### <a name="azure-data-lake-storage-gen2-as-a-source-type"></a>Azure Data Lake Storage Gen2 作为源类型

复制活动源部分支持以下属性：

| 属性 | 说明 | 必选 |
|:--- |:--- |:--- |
| type | 复制活动源的 type 属性必须设置为 AzureBlobFSSource。 |是 |
| recursive | 指示是要从子文件夹中以递归方式读取数据，还是只从指定的文件夹中读取数据。 请注意，当 recursive 设置为 true 且接收器是基于文件的存储时，将不会在接收器上复制或创建空的文件夹或子文件夹。<br/>允许的值为 **true**（默认值）和 **false**。 | 否 |

**示例：**

```json
"activities":[
    {
        "name": "CopyFromAzureDataLakeStorage",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<Azure Data Lake Storage input dataset name>",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "<output dataset name>",
                "type": "DatasetReference"
            }
        ],
        "typeProperties": {
            "source": {
                "type": "AzureBlobFSSource",
                "recursive": true
            },
            "sink": {
                "type": "<sink type>"
            }
        }
    }
]
```

### <a name="azure-data-lake-storage-gen2-as-a-sink-type"></a>Azure Data Lake Storage Gen2 作为接收器类型

复制活动接收器部分中支持以下属性：

| 属性 | 说明 | 必选 |
|:--- |:--- |:--- |
| type | 复制活动接收器的 type 属性必须设置为 AzureBlobFSSink。 |是 |
| copyBehavior | 定义以基于文件的数据存储中的文件为源时的复制行为。<br/><br/>允许值包括：<br/><b>- PreserveHierarchy（默认）</b>：将文件层次结构保留到目标文件夹中。 从源文件到源文件夹的相对路径与从目标文件到目标文件夹的相对路径相同。<br/><b>- FlattenHierarchy</b>：源文件夹中的所有文件都位于目标文件夹的第一级中。 目标文件具有自动生成的名称。 <br/><b>- MergeFiles</b>：将源文件夹中的所有文件合并到一个文件中。 如果指定了文件名，则合并文件的名称为指定名称。 否则，它是自动生成的文件名。 | 否 |

**示例：**

```json
"activities":[
    {
        "name": "CopyToAzureDataLakeStorage",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<input dataset name>",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "<Azure Data Lake Storage Gen2 output dataset name>",
                "type": "DatasetReference"
            }
        ],
        "typeProperties": {
            "source": {
                "type": "<source type>"
            },
            "sink": {
                "type": "AzureBlobFSSink",
                "copyBehavior": "PreserveHierarchy"
            }
        }
    }
]
```

### <a name="folder-and-file-filter-examples"></a>文件夹和文件筛选器示例

本部分介绍使用通配符筛选器生成文件夹路径和文件名的行为。

| folderPath | fileName | recursive | 源文件夹结构和筛选器结果（用**粗体**表示的文件已检索）|
|:--- |:--- |:--- |:--- |
| `Folder*` | （为空，使用默认值） | false | FolderA<br/>&nbsp;&nbsp;&nbsp;&nbsp;**File1.csv**<br/>&nbsp;&nbsp;&nbsp;&nbsp;**File2.json**<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File3.csv<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4.json<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5.csv<br/>AnotherFolderB<br/>&nbsp;&nbsp;&nbsp;&nbsp;File6.csv |
| `Folder*` | （为空，使用默认值） | true | FolderA<br/>&nbsp;&nbsp;&nbsp;&nbsp;**File1.csv**<br/>&nbsp;&nbsp;&nbsp;&nbsp;**File2.json**<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**File3.csv**<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**File4.json**<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**File5.csv**<br/>AnotherFolderB<br/>&nbsp;&nbsp;&nbsp;&nbsp;File6.csv |
| `Folder*` | `*.csv` | false | FolderA<br/>&nbsp;&nbsp;&nbsp;&nbsp;**File1.csv**<br/>&nbsp;&nbsp;&nbsp;&nbsp;File2.json<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File3.csv<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4.json<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5.csv<br/>AnotherFolderB<br/>&nbsp;&nbsp;&nbsp;&nbsp;File6.csv |
| `Folder*` | `*.csv` | true | FolderA<br/>&nbsp;&nbsp;&nbsp;&nbsp;**File1.csv**<br/>&nbsp;&nbsp;&nbsp;&nbsp;File2.json<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**File3.csv**<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4.json<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**File5.csv**<br/>AnotherFolderB<br/>&nbsp;&nbsp;&nbsp;&nbsp;File6.csv |

### <a name="some-recursive-and-copybehavior-examples"></a>一些 recursive 和 copyBehavior 示例

本节介绍了将 recursive 和 copyBehavior 值进行不同组合所产生的复制操作行为。

| recursive | copyBehavior | 源文件夹结构 | 生成目标 |
|:--- |:--- |:--- |:--- |
| true |preserveHierarchy | Folder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5 | 使用与源相同的结构创建目标文件夹 Folder1：<br/><br/>Folder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5 |
| true |flattenHierarchy | Folder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5 | 使用以下结构创建目标 Folder1： <br/><br/>Folder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File1 的自动生成的名称<br/>&nbsp;&nbsp;&nbsp;&nbsp;File2 的自动生成的名称<br/>&nbsp;&nbsp;&nbsp;&nbsp;File3 的自动生成的名称<br/>&nbsp;&nbsp;&nbsp;&nbsp;File4 的自动生成的名称<br/>&nbsp;&nbsp;&nbsp;&nbsp;File5 的自动生成的名称 |
| true |mergeFiles | Folder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5 | 使用以下结构创建目标 Folder1： <br/><br/>Folder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File1 + File2 + File3 + File4 + File5 的内容将合并到一个文件中，且自动生成文件名。 |
| false |preserveHierarchy | Folder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5 | 使用以下结构创建目标文件夹 Folder1： <br/><br/>Folder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File2<br/><br/>不会选取带有 File3、File4 和 File5 的 Subfolder1。 |
| false |flattenHierarchy | Folder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5 | 使用以下结构创建目标文件夹 Folder1： <br/><br/>Folder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File1 的自动生成的名称<br/>&nbsp;&nbsp;&nbsp;&nbsp;File2 的自动生成的名称<br/><br/>不会选取带有 File3、File4 和 File5 的 Subfolder1。 |
| false |mergeFiles | Folder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5 | 使用以下结构创建目标文件夹 Folder1<br/><br/>Folder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File1 + File2 的内容将合并到一个文件中，且自动生成文件名。 File1 的自动生成的名称<br/><br/>不会选取带有 File3、File4 和 File5 的 Subfolder1。 |

## <a name="next-steps"></a>后续步骤

有关数据工厂中复制活动支持作为源和接收器的数据存储的列表，请参阅[支持的数据存储](copy-activity-overview.md##supported-data-stores-and-formats)。
