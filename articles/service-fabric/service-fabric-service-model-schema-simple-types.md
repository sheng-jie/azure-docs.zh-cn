---
title: Azure Service Fabric 服务模型 XML 架构简单类型 | Microsoft Docs
description: 介绍 Service Fabric 服务模型的 XML 架构中的简单类型。
services: service-fabric
documentationcenter: na
author: rwike77
manager: timlt
editor: ''
ms.assetid: ''
ms.service: service-fabric
ms.devlang: xml
ms.topic: reference
ms.tgt_pltfrm: na
ms.workload: multiple
ms.date: 12/10/2018
ms.author: ryanwi
ms.openlocfilehash: d50cdf72cd4de69f65cdc25e3228f6142e75a46f
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/18/2019
ms.locfileid: "57877099"
---
<!-- This article was generated by the Python script found in the service-fabric-service-model-schema.md file -->

# <a name="service-model-xml-schema-simple-types"></a>服务模型 XML 架构简单类型

## <a name="fabricuri-simpletype"></a>FabricUri simpleType
Windows Fabric 命名系统将其用作服务的稳定标识符的 URI。 

### <a name="xml-source"></a>XML 源
```xml
<xs:simpleType xmlns:xs="https://www.w3.org/2001/XMLSchema" xmlns="http://schemas.microsoft.com/2011/01/fabric" name="FabricUri">
    <xs:annotation>
      <xs:documentation>A URI that is used as a stable identifier of services by Microsoft Azure Service Fabric Naming system. </xs:documentation>
    </xs:annotation>
    <xs:restriction base="xs:anyURI"/>
  </xs:simpleType>
  
```
