---
title: 教程：Azure Active Directory 与 Sansan 集成 | Microsoft Docs
description: 了解如何在 Azure Active Directory 和 Sansan 之间配置单一登录。
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.assetid: f653a0f2-c44a-4670-b936-68c136b578ea
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/16/2018
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 8a4e640ba87adf54363611a708b1fec9b00b99e3
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/13/2019
ms.locfileid: "56168217"
---
# <a name="tutorial-azure-active-directory-integration-with-sansan"></a>教程：Azure Active Directory 与 Sansan 集成

本教程介绍如何将 Sansan 与 Azure Active Directory (Azure AD) 集成。

将 Sansan 与 Azure AD 集成可提供以下优势：

- 可在 Azure AD 中控制谁有权访问 Sansan
- 可让用户使用其 Azure AD 帐户自动登录到 Sansan（单一登录）
- 可以在一个中心位置（即 Azure 门户）管理帐户

如需了解有关 SaaS 应用与 Azure AD 集成的详细信息，请参阅 [Azure Active Directory 的应用程序访问与单一登录是什么](../manage-apps/what-is-single-sign-on.md)。

## <a name="prerequisites"></a>先决条件

若要配置 Azure AD 与 Sansan 的集成，需要以下项：

- Azure AD 订阅
- 已启用 Sansan 单一登录的订阅

> [!NOTE]
> 为了测试本教程中的步骤，我们不建议使用生产环境。

测试本教程中的步骤应遵循以下建议：

- 除非必要，请勿使用生产环境。
- 如果没有 Azure AD 试用环境，可以在[此处](https://azure.microsoft.com/pricing/free-trial/)获取一个月的试用版。

## <a name="scenario-description"></a>方案描述
在本教程中，将在测试环境中测试 Azure AD 单一登录。 本教程中概述的方案包括两个主要构建基块：

1. 从库中添加 Sansan
2. 配置和测试 Azure AD 单一登录

## <a name="adding-sansan-from-the-gallery"></a>从库中添加 Sansan
若要配置 Sansan 与 Azure AD 的集成，需要从库中将 Sansan 添加到托管 SaaS 应用列表。

若要从库中添加 Sansan，请执行以下步骤：

1. 在 **[Azure 门户](https://portal.azure.com)** 的左侧导航面板中，单击“Azure Active Directory”图标。 

    ![Active Directory][1]

2. 导航到“企业应用程序”。 然后转到“所有应用程序”。

    ![应用程序][2]
    
3. 若要添加新应用程序，请单击对话框顶部的“新建应用程序”按钮。

    ![应用程序][3]

4. 在搜索框中，键入“Sansan”。

    ![创建 Azure AD 测试用户](./media/sansan-tutorial/tutorial_sansan_search.png)

5. 在结果面板中，选择“Sansan”，并单击“添加”按钮添加该应用程序。

    ![创建 Azure AD 测试用户](./media/sansan-tutorial/tutorial_sansan_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>配置和测试 Azure AD 单一登录
在本部分中，基于一个名为“Britta Simon”的测试用户使用 Sansan 配置和测试 Azure AD 单一登录。

为使单一登录能正常工作，Azure AD 需要知道 Sansan 中与 Azure AD 用户对应的用户。 换句话说，需要在 Azure AD 用户与 Sansan 中相关用户之间建立链接关系。

可通过将 Azure AD 中“用户名”的值指定为 Sansan 中“用户名”的值来建立此链接关系。

若要配置和测试 Sansan 的 Azure AD 单一登录，需要完成以下构建基块：

1. **[配置 Azure AD 单一登录](#configuring-azure-ad-single-sign-on)** - 让用户使用此功能。
2. **[创建 Azure AD 测试用户](#creating-an-azure-ad-test-user)** - 使用 Britta Simon 测试 Azure AD 单一登录。
3. [创建 Sansan 测试用户](#creating-a-sansan-test-user) - 在 Sansan 中创建 Britta Simon 的对应用户，并将其链接到用户的 Azure AD 身份。
4. **[分配 Azure AD 测试用户](#assigning-the-azure-ad-test-user)** - 让 Britta Simon 使用 Azure AD 单一登录。
5. **[测试单一登录](#testing-single-sign-on)** - 验证配置是否正常工作。

### <a name="configuring-azure-ad-single-sign-on"></a>配置 Azure AD 单一登录

本部分介绍如何在 Azure 门户中启用 Azure AD 单一登录并在 Sansan 应用程序中配置单一登录。

若要配置 Sansan 的 Azure AD 单一登录，请执行以下步骤：

1. 在 Azure 门户中的 Sansan 应用程序集成页上，单击“单一登录”。

    ![配置单一登录][4]

2. 在“单一登录”对话框中，选择“基于 SAML 的单一登录”作为“模式”以启用单一登录。
 
    ![配置单一登录](./media/sansan-tutorial/tutorial_sansan_samlbase.png)

3. 在“Sansan 域和 URL”部分中，执行以下步骤：

    ![配置单一登录](./media/sansan-tutorial/tutorial_sansan_url.png)

    在“登录 URL”文本框中，使用以下模式键入 URL： 
    
    | 环境 | 代码 |
    |:--- |:--- |
    | 电脑 Web |`https://ap.sansan.com/v/saml2/<company name>/acs` |
    | 本机移动应用 |`https://internal.api.sansan.com/saml2/<company name>/acs` |
    | 移动浏览器设置 |`https://ap.sansan.com/s/saml2/<company name>/acs` |  

    > [!NOTE] 
    > 这些不是实际值。 请使用实际登录 URL 更新这些值。 请联系 [Sansan 客户端支持团队](https://www.sansan.com/form/contact)获取这些值。 
     
4. 在“SAML 签名证书”部分中，单击“证书(base64)”，并在计算机上保存证书文件。

    ![配置单一登录](./media/sansan-tutorial/tutorial_sansan_certificate.png) 

5. 单击“保存”按钮。

    ![配置单一登录](./media/sansan-tutorial/tutorial_general_400.png)

6. Sansan 应用程序需要多个标识符和答复 URL 来支持多个环境（电脑 Web、本机移动应用、移动浏览器设置），可使用 PowerShell 脚本进行配置。 下面介绍了详细步骤。

7. 若要为使用 PowerShell 脚本的 Sansan 应用程序配置多个标识符和答复 URL，请执行以下步骤：

    ![配置单一登录对象](./media/sansan-tutorial/tutorial_sansan_objid.png)  

    a. 转到“Sansan”应用程序的“属性”页，使用“复制”按钮复制对象 ID 并将其粘贴到记事本中。

    b. 从 Azure 门户复制的对象 ID 将在本教程后面用作 PowerShell 脚本中的 ServicePrincipalObjectId。 

    c. 现在打开提升的 Windows PowerShell 命令提示符。
    
    >[!NOTE] 
    > 需要安装 AzureAD 模块（使用命令 `Install-Module -Name AzureAD`）。 出现安装 NuGet 模块或新的 Azure Active Directory V2 PowerShell 模块的提示时，请键入 Y，然后按 ENTER。

    d. 运行 `Connect-AzureAD` 并使用全局管理员用户帐户登录。

    e. 使用以下脚本向应用程序更新多个 URL：

    ```powershell
     Param(
    [Parameter(Mandatory=$true)][guid]$ServicePrincipalObjectId,
    [Parameter(Mandatory=$false)][string[]]$ReplyUrls,
    [Parameter(Mandatory=$false)][string[]]$IdentifierUrls
    )

    $servicePrincipal = Get-AzureADServicePrincipal -ObjectId $ServicePrincipalObjectId

    if($ReplyUrls.Length)
    {
    echo "Updating Reply urls"
    Set-AzureADServicePrincipal -ObjectId $ServicePrincipalObjectId -ReplyUrls $ReplyUrls
    echo "updated"
    }
    if($IdentifierUrls.Length)
    {
    echo "Updating Identifier urls"
    $applications = Get-AzureADApplication -SearchString $servicePrincipal.AppDisplayName 
    echo "Found Applications =" $applications.Length
    $i = 0;
    do
    {  
    $application = $applications[$i];
    if($application.AppId -eq $servicePrincipal.AppId){
    Set-AzureADApplication -ObjectId $application.ObjectId -IdentifierUris $IdentifierUrls
    $servicePrincipal = Get-AzureADServicePrincipal -ObjectId $ServicePrincipalObjectId
    echo "Updated"
    return;
    }
    $i++;
    }while($i -lt $applications.Length);
    echo "Not able to find the matched application with this service principal"
    }
    ```

8. 成功完成 PowerShell 脚本后，脚本的结果将如下所示，且 URL 值将更新，但它们不会反映在 Azure 门户中。 

    ![配置单一登录脚本](./media/sansan-tutorial/tutorial_sansan_powershell.png)


9. 在“Sansan 配置”部分中，单击“配置 Sansan”，打开“配置登录”窗口。 从“快速参考”部分中复制“注销 URL”、“SAML 实体 ID”和“SAML 单一登录服务 URL”。

    ![配置单一登录](./media/sansan-tutorial/tutorial_sansan_configure.png) 

10. 若要在 Sansan 端配置单一登录，需要将下载的证书、注销 URL、SAML 实体 ID 和 SAML 单一登录服务 URL 发送给 [Sansan 支持团队](https://www.sansan.com/form/contact)。 他们会对此进行设置，使两端的 SAML SSO 连接均正确设置。

>[!NOTE]
>电脑浏览器设置也适用于移动应用和移动浏览器以及电脑 Web。 

### <a name="creating-an-azure-ad-test-user"></a>创建 Azure AD 测试用户

本部分的目的是在 Azure 门户中创建名为 Britta Simon 的测试用户。

![创建 Azure AD 用户][100]

**若要在 Azure AD 中创建测试用户，请执行以下步骤：**

1. 在 **Azure 门户**的左侧导航窗格中，单击“Azure Active Directory”图标。

    ![创建 Azure AD 测试用户](./media/sansan-tutorial/create_aaduser_01.png) 

2. 若要显示用户列表，请转到“用户和组”，单击“所有用户”。
    
    ![创建 Azure AD 测试用户](./media/sansan-tutorial/create_aaduser_02.png) 

3. 若要打开“用户”对话框，请在对话框顶部单击“添加”。
 
    ![创建 Azure AD 测试用户](./media/sansan-tutorial/create_aaduser_03.png) 

4. 在“用户”对话框页上，执行以下步骤：
 
    ![创建 Azure AD 测试用户](./media/sansan-tutorial/create_aaduser_04.png) 

    a. 在“名称”文本框中，键入 **BrittaSimon**。

    b. 在“用户名”文本框中，键入 BrittaSimon 的“电子邮件地址”。

    c. 选择“显示密码”并记下“密码”的值。

    d. 单击“创建”。
 
### <a name="creating-a-sansan-test-user"></a>创建 Sansan 测试用户

在本部分中，会在 Sansan 中创建一个名为“Britta Simon”的用户。 在执行 SSO 前，Sansan 应用程序需要在应用程序中预配用户。 

>[!NOTE]
>如果需要手动创建一个用户或一批用户，请联系 [Sansan 支持团队](https://www.sansan.com/form/contact)。 

### <a name="assigning-the-azure-ad-test-user"></a>分配 Azure AD 测试用户

在本部分中，通过授予 Britta Simon 访问 Sansan 的权限，允许她使用 Azure 单一登录。

![分配用户][200] 

若要将 Britta Simon 分配到 Sansan，请执行以下步骤：

1. 在 Azure 门户中打开应用程序视图，导航到目录视图，接着转到“企业应用程序”，并单击“所有应用程序”。

    ![分配用户][201] 

2. 在应用程序列表中，选择“Sansan”。

    ![配置单一登录](./media/sansan-tutorial/tutorial_sansan_app.png) 

3. 在左侧菜单中，单击“用户和组”。

    ![分配用户][202] 

4. 单击“添加”按钮。 然后在“添加分配”对话框中选择“用户和组”。

    ![分配用户][203]

5. 在“用户和组”对话框的“用户”列表中，选择“Britta Simon”。

6. 在“用户和组”对话框中单击“选择”按钮。

7. 在“添加分配”对话框中单击“分配”按钮。
    
### <a name="testing-single-sign-on"></a>测试单一登录

在本部分中，使用访问面板测试 Azure AD 单一登录配置。

单击访问面板中的“Sansan”磁贴时，应自动登录到 Sansan 应用程序。
有关访问面板的详细信息，请参阅 [Introduction to the Access Panel](../user-help/active-directory-saas-access-panel-introduction.md)（访问面板简介）。

## <a name="additional-resources"></a>其他资源

* [有关如何将 SaaS 应用与 Azure Active Directory 集成的教程列表](tutorial-list.md)
* [Azure Active Directory 的应用程序访问与单一登录是什么？](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/sansan-tutorial/tutorial_general_01.png
[2]: ./media/sansan-tutorial/tutorial_general_02.png
[3]: ./media/sansan-tutorial/tutorial_general_03.png
[4]: ./media/sansan-tutorial/tutorial_general_04.png

[100]: ./media/sansan-tutorial/tutorial_general_100.png

[200]: ./media/sansan-tutorial/tutorial_general_200.png
[201]: ./media/sansan-tutorial/tutorial_general_201.png
[202]: ./media/sansan-tutorial/tutorial_general_202.png
[203]: ./media/sansan-tutorial/tutorial_general_203.png

