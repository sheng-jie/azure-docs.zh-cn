---
title: 从 Azure 市场部署 VM | Microsoft Docs
description: 介绍如何部署虚拟机从 Azure Marketplace 预配置虚拟机。
services: Azure, Marketplace, Cloud Partner Portal,
documentationcenter: ''
author: v-miclar
manager: Patrick.Butler
editor: ''
ms.assetid: ''
ms.service: marketplace
ms.workload: ''
ms.tgt_pltfrm: ''
ms.devlang: ''
ms.topic: article
ms.date: 11/29/2018
ms.author: pbutlerm
ms.openlocfilehash: d800d2a9c4eced2fa347658ecbb5b7a97031d997
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/18/2019
ms.locfileid: "57838691"
---
# <a name="deploy-a-virtual-machine-from-the-azure-marketplace"></a>从 Azure 市场部署虚拟机

本文介绍如何使用提供的 Azure PowerShell 脚本从 Azure 市场部署预配置的虚拟机 (VM)。  此脚本还公开 VM 上的 WinRM HTTP 和 HTTPS 终结点。  它需要你已按照[创建 Azure Key Vault 证书](./cpp-create-key-vault-cert.md)中所述，将证书上传到了 Azure Key Vault。 


## <a name="vm-deployment-template"></a>VM 部署模板

Azure VM 部署快速入门模板现以联机文件 [azuredeploy.json](https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/201-vm-winrm-keyvault-windows/azuredeploy.json) 形式提供。  它包含以下参数：

|  **Parameter**        |   **说明**                                 |
|  -------------        |   ---------------                                 |
| newStorageAccountName | 存储帐户的名称                       |
| dnsNameForPublicIP    | 公共 IP 的 DNS 名称。 必须为小写。    |
| adminUserName         | 管理员的用户名                          |
| adminPassword         | 管理员的密码                          |
| imagePublisher        | 映像发布者                                   |
| imageOffer            | 映像产品/服务                                       |
| imageSKU              | 映像 SKU                                         |
| vmSize                | VM 的大小                                    |
| vmName                | VM 的名称                                    |
| vaultName             | 密钥保管库的名称                             |
| vaultResourceGroup    | 密钥保管库的资源组                   |
| certificateUrl        | 证书的 URL，包括KeyVault 中的版本，例如：`https://testault.vault.azure.net/secrets/testcert/b621es1db241e56a72d037479xab1r7` |
|  |  |


## <a name="deployment-script"></a>部署脚本

编辑并执行以下 Azure PowerShell 脚本，以部署指定的 Azure 市场 VM。

```powershell

New-AzureRmResourceGroupDeployment -Name "dplvm$postfix" -ResourceGroupName "$rgName" -TemplateUri "https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/201-vm-winrm-keyvault-windows/azuredeploy.json" -newStorageAccountName "test$postfix" -dnsNameForPublicIP $vmName -adminUserName "isv" -adminPassword $pwd -vmSize "Standard_A2" -vmName $vmName -vaultName "$kvname" -vaultResourceGroup "$rgName" -certificateUrl $objAzureKeyVaultSecret.Id 

```


## <a name="next-steps"></a>后续步骤

部署了预配置的 VM 后，即可配置并访问它所含的解决方案和服务，或将它用于进一步开发。 
