---
title: Azure 开发测试实验室常见问题解答 | Microsoft Docs
description: 查找 Azure 开发测试实验室相关常见问题的答案。
services: devtest-lab,virtual-machines,lab-services
documentationcenter: na
author: spelluru
manager: femila
editor: ''
ms.assetid: afe83109-b89f-4f18-bddd-b8b4a30f11b4
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/21/2019
ms.author: spelluru
ms.openlocfilehash: 3752e9c227e4db0f583b2f6b21d6c0aa3106d248
ms.sourcegitcommit: 02d17ef9aff49423bef5b322a9315f7eab86d8ff
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/21/2019
ms.locfileid: "58337691"
---
# <a name="azure-devtest-labs-faq"></a>Azure 开发测试实验室常见问题
获取关于 Azure 开发测试实验室的某些最常见问题的解答。

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

**常规**

## <a name="blog-post"></a>博客文章
已从 20 年 3 月 2019年开始停用我们的开发测试实验室团队博客。 

### <a name="where-can-i-track-feature-updates-going-forward"></a>在其中跟踪今后的功能更新？
今后，我们将发布功能更新和/或信息性的博客文章 Azure 博客上和 Azure 更新。 任何所需位置，这些博客文章还将链接到我们的文档。

订阅[开发测试实验室 Azure 博客](https://azure.microsoft.com/blog/tag/azure-devtest-labs/)并[开发测试实验室 Azure 更新](https://azure.microsoft.com/updates/?product=devtest-lab)以便随时了解开发测试实验室中的新增功能。

### <a name="what-happens-to-the-existing-blog-posts"></a>现有的博客文章会发生什么情况？
我们目前正在迁移现有的博客文章 （不包括中断更新） 到我们[开发测试实验室文档](devtest-lab-overview.md)。 MSDN 博客已被弃用，它会将重定向到文档概述用于开发测试实验室。 一旦重定向，您可以搜索您正在寻找筛选器的标题中的文章。 请注意，我们还没有迁移所有帖子，但应通过此月结束。 


### <a name="where-do-i-see-outage-updates"></a>其中看中断更新？
我们将发布中断更新使用今后我们 Twitter 句柄。 若要获取最新信息更新中断和已知的 bug 在 Twitter 上关注我们。

### <a name="twitter"></a>Twitter 
我们的 twitter: azlabservices

## <a name="what-if-my-question-isnt-answered-here"></a>如果未在此处找到相关问题怎么办？
如果此处未列出您的问题，让我们知道，并且我们可以帮助您找到答案。

* 在本常见问题解答的末尾发布问题。 与 Azure 缓存团队和其他社区成员就本文进行讨论。
* 若希望更多人看到问题，可以将问题发布在 [Azure 开发测试实验室 MSDN 论坛](https://social.msdn.microsoft.com/Forums/azure/home?forum=AzureDevTestLabs)上。 与 Azure 开发测试实验室团队和其他社区成员进行讨论。
* 要提出功能请求，请将请求和想法提交到 [Azure DevTest Labs User Voice](https://feedback.azure.com/forums/320373-azure-devtest-labs)（Azure 开发测试实验室用户之声）。

## <a name="why-should-i-use-azure-devtest-labs"></a>为什么要使用 Azure 开发测试实验室？
Azure 开发测试实验室可为团队节省时间和金钱。 开发人员可使用几个不同的基项创建自己的环境。 还可使用项目快速部署和配置应用程序。 使用自定义映像和公式，可将虚拟机 (VM) 保存为模板，并在团队中轻松重现。 开发测试实验室还提供多个可配置策略，可供实验室管理员用于减少浪费和管理团队的环境。 这些策略包括自动关机、成本阈值、每个用户的最大 VM 数量和最大 VM 大小。 有关开发测试实验室更深入的说明，请参阅[概述](devtest-lab-overview.md)或观看[介绍视频](https://channel9.msdn.com/Blogs/Azure/what-is-azure-devtest-labs)。

## <a name="what-does-worry-free-self-service-mean"></a>“无忧的自助服务”是什么意思？
“无忧的自助服务”是指开发人员和测试人员按需创建自己的环境。 管理员知道 Azure 开发测试实验室可帮助尽量减少浪费和控制成本，因而感到安心无忧。 管理员可以指定允许的 VM 大小、最大 VM 数量以及 VM 的启动和关机时间。 开发测试实验室还可简化成本监视和警报设置，可帮助用户始终注意实验室资源的使用方式。

## <a name="how-can-i-use-devtest-labs"></a>如何使用开发测试实验室？
开发测试实验室都非常有帮助你需要开发或测试环境，并想要快速，重现或使用节省成本的策略管理。

下面列出了客户使用开发测试实验室的某些情形：

* 在一个位置管理开发和测试环境。 使用策略来降低成本和创建自定义映像，以便在团队中共享生成。
* 使用自定义映像开发应用程序，以便保存整个开发阶段的磁盘状态。
* 跟踪与进度相关的成本。
* 为质量保证测试创建大容量测试环境。
* 使用项目和公式轻松在各种环境中配置和重现应用程序。
* 为编程马拉松分发 VM（协作开发或测试工作），并在事件结束时轻松对其取消预配。

## <a name="how-am-i-billed-for-devtest-labs"></a>开发测试实验室如何计费？
开发测试实验室是一项免费服务。 可免费在开发测试实验室中创建实验室，配置策略、模板和项目。 只需为在实验室中使用的 Azure 资源（例如 VM、存储帐户和虚拟网络）付费。 有关实验室资源费用的详细信息，请参阅 [Azure 开发测试实验室定价](https://azure.microsoft.com/pricing/details/devtest-lab/)。


**安全性**
## <a name="what-are-the-different-security-levels-in-devtest-labs"></a>开发测试实验室中有哪些安全级别？
安全访问权限由[基于角色的访问控制 (RBAC)](../role-based-access-control/built-in-roles.md) 决定。 要了解访问权限的工作方式，最好先了解 RBAC 定义的权限、角色与作用域之间的差异。

* **权限**：权限是针对特定操作定义的访问权限。 例如，权限可以是对所有 VM 的读取访问权限。
* **角色**：角色是可进行分组和分配给用户的一组权限。 例如，具有“订阅所有者”角色的用户对订阅中所有资源都具有访问权限。
* **范围**：作用域是 Azure 资源的层次结构中的一个级别。 例如，作用域可以是资源组、单个实验室或整个订阅。

开发测试实验室的作用域中有两种定义用户权限的角色：

* **实验室所有者**：实验室所有者对实验室中的所有资源都具有访问权限。 实验室所有者可进行修改策略、读取和写入任意 VM、更改虚拟网络等操作。
* **实验室用户**：实验室用户可查看所有实验室资源（例如 VM、策略和虚拟网络）。 但是，实验室用户不能修改其他用户创建的策略和 VM。 

还可以在开发测试实验室中创建自定义角色。 要了解如何在开发测试实验室中创建自定义角色，请参阅[向用户授予特定实验室策略的权限](devtest-lab-grant-user-permissions-to-specific-lab-policies.md)。

由于作用域是分层的，因此在用户具有某个作用域的权限时，系统会自动向该用户授予该作用域内每个较低级别的作用域的权限。 例如，如果向用户分配了订阅所有者角色，那么用户可访问订阅中的所有资源。 这些资源包括所有 VM、所有虚拟网络和所有实验室。 订阅所有者会自动继承实验室所有者的角色。 但是反过来则不适用。 实验室所有者具有访问实验室的权限，其低于订阅级别的作用域。 因此，实验室所有者不能查看 VM、虚拟网络或实验室之外的任何其他资源。

## <a name="how-do-i-create-a-role-to-allow-users-to-perform-a-specific-task"></a>如何通过创建角色来允许用户执行特定任务？
有关如何创建自定义角色，并将权限分配给该角色的综合性文章，请参阅[向用户授予特定实验室策略的权限](devtest-lab-grant-user-permissions-to-specific-lab-policies.md)。 下面的脚本示例创建“开发测试实验室高级用户”角色，该角色具有启动和停止实验室中所有 VM 的权限：

    $policyRoleDef = Get-AzRoleDefinition "DevTest Labs User"
    $policyRoleDef.Actions.Remove('Microsoft.DevTestLab/Environments/*')
    $policyRoleDef.Id = $null
    $policyRoleDef.Name = "DevTest Labs Advanced User"
    $policyRoleDef.IsCustom = $true
    $policyRoleDef.AssignableScopes.Clear()
    $policyRoleDef.AssignableScopes.Add("subscriptions/<subscription Id>")
    $policyRoleDef.Actions.Add("Microsoft.DevTestLab/labs/virtualMachines/Start/action")
    $policyRoleDef.Actions.Add("Microsoft.DevTestLab/labs/virtualMachines/Stop/action")
    $policyRoleDef = New-AzRoleDefinition -Role $policyRoleDef  


**CI/CD 集成和自动化**
## <a name="does-devtest-labs-integrate-with-my-cicd-toolchain"></a>开发测试实验室可与我的 CI/CD 工具链集成吗？
如果使用的是 Azure DevOps，则可使用[开发测试实验室任务扩展](https://marketplace.visualstudio.com/items?itemName=ms-azuredevtestlabs.tasks)自动化开发测试实验室的发布管道。 可使用此扩展执行的任务包括：

* 自动创建和部署 VM。 还可以使用 Azure 文件复制或 PowerShell Azure DevOps Services 任务配置具有最新生成的 VM。
* 测试后自动获取 VM 的状态以在同一 VM 中再现 Bug 进行进一步调查。
* 当不再需要时，删除发布管道末尾的 VM。

以下博客文章提供了有关使用 Azure DevOps Services 扩展的指南和信息：

* [开发测试实验室和 Azure DevOps 扩展](https://blogs.msdn.microsoft.com/devtestlab/2016/06/15/azure-devtest-labs-vsts-extension/)
* [在现有开发测试实验室中从 Azure DevOps Services 部署新 VM](https://www.visualstudiogeeks.com/blog/DevOps/Deploy-New-VM-To-Existing-AzureDevTestLab-From-VSTS)
* [使用Azure DevOps Services 发布管理对开发测试实验室进行持续部署](https://www.visualstudiogeeks.com/blog/DevOps/Use-VSTS-ReleaseManagement-to-Deploy-and-Test-in-AzureDevTestLabs)

对于其他持续集成 (CI)/持续交付 (CD) 工具链，可通过使用 [Azure PowerShell cmdlet](../azure-resource-manager/resource-group-template-deploy.md) 和 [.NET SDK](https://www.nuget.org/packages/Microsoft.Azure.Management.DevTestLabs/) 部署 [Azure 资源管理器模板](https://aka.ms/dtlquickstarttemplate)来实现相同的方案。 还可使用[用于开发测试实验室的 REST API](https://aka.ms/dtlrestapis) 与工具链进行集成。  


**虚拟机**
## <a name="why-cant-i-see-vms-on-the-virtual-machines-page-that-i-see-in-devtest-labs"></a>为什么无法看到我在开发测试实验室中看到的虚拟机页上的 Vm？
在开发测试实验室中创建 VM 时，用户具有访问该 VM 的权限。 您可以查看 VM 的实验室上并在**虚拟机**页。 分配到开发测试实验室实验室用户角色的用户可以看到在实验室的实验室中创建的所有 Vm**的所有虚拟机**页。 但是，系统不会自动向具有“开发测试实验室”实验室用户角色的用户授予对其他用户创建的 VM 资源的读取访问权限。 因此，这些 Vm 不会显示在**虚拟机**页。

## <a name="what-is-the-difference-between-a-custom-image-and-a-formula"></a>自定义映像与公式之间的区别是什么？
自定义映像是一个虚拟硬盘 (VHD)。 而公式是可使用其他设置配置，并在随后保存和重现的映像。 要使用同一不可变的基本映像快速创建多个环境，最好使用自定义映像。 要使用最新位（作为虚拟网络或子网的一部分，或作为特定大小的 VM）重现 VM 的配置，选择公式可能会更好。 有关更深入的说明，请参阅[比较自定义映像和 DevTest 实验室中的公式](devtest-lab-comparing-vm-base-image-types.md)。

## <a name="how-do-i-create-multiple-vms-from-the-same-template-at-once"></a>如何一次性从同一模板创建多个 VM？
可通过两种方式同时从同一模板创建多个 VM：
* 可以使用 [Azure DevOps 任务扩展](https://marketplace.visualstudio.com/items?itemName=ms-azuredevtestlabs.tasks)。 
* 创建 VM 和[从 Windows PowerShell 部署资源管理器模板](../azure-resource-manager/resource-group-template-deploy.md)时，可[生成资源管理器模板](devtest-lab-add-vm.md#save-azure-resource-manager-template)。

## <a name="how-do-i-move-my-existing-azure-vms-into-my-devtest-labs-lab"></a>如何将现有 Azure VM 移到开发测试实验室？
将现有 VM 复制到开发测试实验室：

1. 使用此 [Windows PowerShell 脚本](https://github.com/Azure/azure-devtestlab/blob/master/Scripts/CopyVHDFromVMToLab.ps1)复制现有 VM 的 VHD 文件。
2. 在开发测试实验室内[创建自定义映像](devtest-lab-create-template.md)。
3. 在实验室中从自定义映像创建 VM。

## <a name="can-i-attach-multiple-disks-to-my-vms"></a>可将多个磁盘附加到我的 VM 中吗？
可以，可将多个磁盘附加到 VM 中。  

## <a name="if-i-want-to-use-a-windows-os-image-for-my-testing-do-i-have-to-purchase-an-msdn-subscription"></a>如果要将 Windows OS 映像用于我的测试，是否需要购买 MSDN 订阅？
要在 Azure 中使用 Windows 客户端 OS 映像（Windows 7 或更高版本）进行开发或测试，必须执行以下操作之一：

- [购买 MSDN 订阅](https://www.visualstudio.com/products/how-to-buy-vs)。
- 如果具有企业协议，请通过 [Enterprise 开发/测试套餐](https://azure.microsoft.com/offers/ms-azr-0148p)创建 Azure 订阅。

若要深入了解每个 MSDN 产品/服务的 Azure 信用额度，请参阅 [Visual Studio 订户的每月 Azure 信用额度](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/)。

## <a name="how-do-i-automate-the-process-of-uploading-vhd-files-to-create-custom-images"></a>如何自动上传 VHD 文件以创建自定义映像？
可通过两种方式自动上传 VHD 文件以创建自定义映像：

* 使用 [AzCopy](../storage/common/storage-use-azcopy.md#upload-blobs-to-blob-storage) 将 VHD 文件复制或上传到与实验室关联的存储帐户。
* 使用 [Azure 存储资源管理器](../vs-azure-tools-storage-manage-with-storage-explorer.md)。 存储资源管理器是在 Windows、OS X 和 Linux 上运行的独立应用。   

查找与实验室关联的目标存储帐户：

1. 登录到 [Azure 门户](https://go.microsoft.com/fwlink/p/?LinkID=525040)。
2. 在左侧菜单上，选择“资源组”。
3. 查找并选择与实验室关联的资源组。
4. 在“概述”下，选择一个存储帐户。
5. 选择“Blob”。
6. 查找列表中的上传。 如果不存在，则返回到步骤 4，并尝试另一个存储帐户。
7. 将该 URL 用作 AzCopy 命令中的目标。

## <a name="how-do-i-automate-the-process-of-deleting-all-the-vms-in-my-lab"></a>如何自动删除实验室中的所有 VM？
可在 Azure 门户中从实验室删除 VM。 还可以使用 PowerShell 脚本删除实验室中的所有 VM。 在下例中，修改“要更改的值”注释下方的参数值。 可以从 Azure 门户的“实验室”窗格中检索 `subscriptionId`、`labResourceGroup` 和 `labName` 值。

    # Delete all the VMs in a lab.

    # Values to change:
    $subscriptionId = "<Enter Azure subscription ID here>"
    $labResourceGroup = "<Enter lab's resource group here>"
    $labName = "<Enter lab name here>"

    # Sign in to your Azure account.
    Connect-AzAccount

    # Select the Azure subscription that has the lab. This step is optional
    # if you have only one subscription.
    Select-AzSubscription -SubscriptionId $subscriptionId

    # Get the lab that has the VMs that you want to delete.
    $lab = Get-AzResource -ResourceId ('subscriptions/' + $subscriptionId + '/resourceGroups/' + $labResourceGroup + '/providers/Microsoft.DevTestLab/labs/' + $labName)

    # Get the VMs from that lab.
    $labVMs = Get-AzResource | Where-Object {
              $_.ResourceType -eq 'microsoft.devtestlab/labs/virtualmachines' -and
              $_.Name -like "$($lab.Name)/*"}

    # Delete the VMs.
    foreach($labVM in $labVMs)
    {
        Remove-AzResource -ResourceId $labVM.ResourceId -Force
    }

**项目**
## <a name="what-are-artifacts"></a>项目是什么？
项目是用于将最新位或开发工具部署到 VM 的可自定义元素。 创建 VM 时，将项目附加到 VM。 预配 VM 后，项目可以部署和配置 VM。 各种预先存在的项目均位于我们[公共 GitHub 存储库](https://github.com/Azure/azure-devtestlab/tree/master/Artifacts)。 也可以[创作自己的项目](devtest-lab-artifact-author.md)。


**实验室配置**
## <a name="how-do-i-create-a-lab-from-a-resource-manager-template"></a>如何从资源管理器模板模板创建实验室？
我们提供了[实验室 Azure 资源管理器模板的 GitHub 存储库](https://aka.ms/dtlquickstarttemplate)，可按原样部署，也可对其进行修改，为实验室创建自定义模板。 每个模板都有一个链接，用于在自己的 Azure 订阅中按原样部署实验室。 或者，可自定义模板，并[使用 PowerShell 或 Azure CLI 进行部署](../azure-resource-manager/resource-group-template-deploy.md)。

## <a name="why-are-my-vms-created-in-different-resource-groups-with-arbitrary-names-can-i-rename-or-modify-these-resource-groups"></a>为什么我的 VM 创建于具有随机名称的不同资源组中？ 我能重新命名或修改这些资源组吗？
资源组的这种创建方式有助于开发测试实验室管理用户权限和对 VM 的访问权限。 虽然可以将 VM 移动到其他资源组并使用所需名称，但建议不要对资源组进行更改。 我们正努力改善该体验，允许更大的灵活性。   

## <a name="how-many-labs-can-i-create-under-the-same-subscription"></a>在同一订阅下，可创建多少个实验室？
没有对每个订阅可创建的实验室数量进行限定。 但对每个订阅使用的资源量进行了限定。 可参阅 [Azure 订阅的限制和配额](../azure-subscription-service-limits.md)和[如何增加这些限制](https://azure.microsoft.com/blog/azure-limits-quotas-increase-requests)。

## <a name="how-many-vms-can-i-create-per-lab"></a>每个实验室可以创建多少个 VM？
没有对每个实验室可创建的 VM 数量进行限定。 但对每个订阅使用的资源（VM 核心数、公共 IP 地址等）进行了限制。 可参阅 [Azure 订阅的限制和配额](../azure-subscription-service-limits.md)和[如何增加这些限制](https://azure.microsoft.com/blog/azure-limits-quotas-increase-requests)。

## <a name="how-do-i-share-a-direct-link-to-my-lab"></a>如何向我的实验室共享直接链接？

1. 在 Azure 门户中，转到实验室。
2. 从浏览器复制实验室 URL 并与实验室用户共享。

> [!NOTE]
> 如果实验室用户是具有 [Microsoft 帐户](#what-is-a-microsoft-account)，但没有贵组织的 Active Directory 实例成员身份的外部用户，则该用户尝试访问共享链接时可能会看到错误消息。 如果外部用户看到错误消息，请让该用户首先在 Azure 门户的右上角选择其名字。 然后，在菜单的目录部分，该用户可选择实验室所在的目录。
>
>

## <a name="what-is-a-microsoft-account"></a>什么是 Microsoft 帐户？
Microsoft 帐户可用于通过 Microsoft 设备和服务执行的几乎所有操作。 它是用于登录 Skype、Outlook.com、OneDrive、Windows Phone 和 Xbox Live 的电子邮件地址和密码。 单一帐户意味着文件、照片、联系人和设置可跟随用户移动到任何设备。

> [!NOTE]
> Microsoft 帐户以前称为“Windows Live ID”。
>
>


**故障排除**
## <a name="my-artifact-failed-during-vm-creation-how-do-i-troubleshoot-it"></a>VM 创建过程中项目失败。 如何解决它？
要了解如何获取失败项目的日志，请参阅[如何诊断开发测试实验室中的项目失败](devtest-lab-troubleshoot-artifact-failure.md)。

## <a name="why-isnt-my-existing-virtual-network-saving-properly"></a>为什么我的虚拟网络没有正确保存？
一种可能是虚拟网络名称包含具句号。 如果包含句号，请尝试将其删除，或将其替换为连字符。 然后再次尝试保存虚拟网络。

## <a name="why-do-i-get-a-parent-resource-not-found-error-when-i-provision-a-vm-from-powershell"></a>从 PowerShell 预配 VM 时，为什么会收到“找不到父资源”错误？
若一个资源是另一个资源的父资源，则该父资源必须存在才可创建子资源。 如果父资源不存在，则会看到 ParentResourceNotFound 消息。 如果未指定与父资源的依赖关系，子资源可能会在父资源之前部署。

VM 是资源组中实验室下的子资源。 使用资源管理器模板通过 PowerShell 部署 VM 时，PowerShell 脚本中提供的资源组名称应为实验室的资源组名称。 有关详细信息，请参阅[排查常见的 Azure 部署错误](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-common-deployment-errors)。

## <a name="where-can-i-find-more-error-information-if-a-vm-deployment-fails"></a>VM 部署失败时，可在何处查找错误详细信息？
活动日志中会捕获 VM 部署错误。 可以找到实验室 VM 活动日志下的**审核日志**或**虚拟机诊断**实验室的虚拟机页上的资源菜单上 (在您选择从 VM 后，会显示的页**我虚拟机**列表)。

有时，部署错误发生在 VM 部署开始之前。 一个示例是超过与 VM 一起创建的资源的订阅限制时。 在此情况下，会在实验室级别活动日志中捕获错误详细信息。 活动日志位于“配置和策略”设置的底部。 若要深入了解如何在 Azure 中使用活动日志，请参阅[查看活动日志以审核对资源的操作](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-audit)。

[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]
