---
title: 自定义 Azure AD 自助服务密码重置-Azure Active Directory
description: 用于 Azure AD 自助服务密码重置的自定义选项
services: active-directory
ms.service: active-directory
ms.subservice: authentication
ms.topic: conceptual
ms.date: 07/11/2018
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: sahenry
ms.collection: M365-identity-device-management
ms.openlocfilehash: d38d93a1c9716cc3a71d904b7b1a46fb8b1c2ee0
ms.sourcegitcommit: 49c8204824c4f7b067cd35dbd0d44352f7e1f95e
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/22/2019
ms.locfileid: "58369218"
---
# <a name="customize-the-azure-ad-functionality-for-self-service-password-reset"></a>为自助密码重置自定义 Azure AD 功能

想要在 Azure Active directory (Azure AD) 中部署自助服务密码重置 (SSPR) 的 IT 专业人员可以自定义与其用户需求相符的体验。

## <a name="customize-the-contact-your-administrator-link"></a>自定义“联系管理员”链接

即使未启用 SSPR，用户在密码重置门户中也仍可找到“联系管理员”链接。 如果用户选择该链接，它将执行以下任务之一：

* 向管理员发送一封电子邮件，请求他们帮助更改用户的密码。
* 将用户指引到指定的 URL 以获取帮助。

建议将此联系人设置为用户已用来提问支持问题的电子邮件地址或网站等内容。

![若要重置电子邮件发送给管理员的示例请求][Contact]

此联系人电子邮件按以下顺序发送到以下收件人：

1. 如果已分配“密码管理员”角色，则会通知充当此角色的管理员。
2. 如果未分配密码管理员，则会通知充当“用户管理员”角色的管理员。
3. 如果上述两个角色都未分配，则会通知“全局管理员”。

在所有情况下，最多会向 100 个收件人发送通知。

若要了解有关不同管理员角色以及如何分配它们的详细信息，请参阅[在 Azure Active Directory 中分配管理员角色](../users-groups-roles/directory-assign-admin-roles.md)。

### <a name="disable-contact-your-administrator-emails"></a>禁用“联系管理员”电子邮件

如果组织不希望向管理员通知密码重置请求，可启用以下配置：

* 为所有最终用户启用自助密码重置。 可在“密码重置” > “属性”下面找到此选项。 如果不希望用户重置其自己的密码，可以将访问权限限制为某个空组。 我们不建议使用此选项。
* 自定义帮助台链接，以提供可让用户获得帮助的 Web URL 或 mailto: 地址。 可在“密码重置” > “自定义” > “自定义支持人员电子邮件或 URL”下面找到此选项。

## <a name="customize-the-ad-fs-sign-in-page-for-sspr"></a>为 SSPR 自定义 AD FS 登录页

Active Directory 联合身份验证服务 (AD FS) 管理员可以使用[添加登录页说明](https://docs.microsoft.com/windows-server/identity/ad-fs/operations/add-sign-in-page-description)一文中的指导将链接添加到登录页。

若要将链接添加到 AD FS 登录页，请在 AD FS 服务器上使用以下命令。 用户可以使用此页输入 SSPR 工作流。

``` powershell
Set-ADFSGlobalWebContent -SigninPageDescriptionText "<p><A href='https://passwordreset.microsoftonline.com' target='_blank'>Can’t access your account?</A></p>"
```

## <a name="customize-the-sign-in-page-and-access-panel-look-and-feel"></a>自定义登录页和访问面板的外观

可以自定义登录页。 可以添加一个徽标，它将随适合公司品牌的图像一起显示。

在以下情况下会显示所选图形：

* 用户输入其用户名后
* 如果用户通过以下方式访问自定义的 URL：
   * 通过传递`whr`到密码重置页，例如 `https://login.microsoftonline.com/?whr=contoso.com`
   * 通过传递`username`到密码重置页，例如 `https://login.microsoftonline.com/?username=admin@contoso.com`

有关如何配置公司品牌的详细信息，请参阅[将公司品牌添加到 Azure AD 中的登录页](../fundamentals/customize-branding.md)一文。

### <a name="directory-name"></a>目录名称

可以在“Azure Active Directory” > “属性”下更改目录名称属性。 可以在门户中以及在自动通信中显示友好的组织名称。 在自动发送的电子邮件中，此选项以下列形式出现时最显眼：

* 电子邮件中的友好名称，例如“Microsoft 代表 CONTOSO 演示”
* 电子邮件中的主题行，例如“CONTOSO 演示帐户电子邮件验证码”

## <a name="next-steps"></a>后续步骤

* [如何成功推出 SSPR？](howto-sspr-deployment.md)
* [重置或更改密码](../user-help/active-directory-passwords-update-your-own-password.md)
* [注册自助密码重置](../user-help/active-directory-passwords-reset-register.md)
* [是否有许可问题？](concept-sspr-licensing.md)
* [SSPR 使用哪些数据？应为用户填充哪些数据？](howto-sspr-authenticationdata.md)
* [哪些身份验证方法可供用户使用？](concept-sspr-howitworks.md#authentication-methods)
* [SSPR 有哪些策略选项？](concept-sspr-policy.md)
* [什么是密码写回？我为什么关心它？](howto-sspr-writeback.md)
* [如何报告 SSPR 中的活动？](howto-sspr-reporting.md)
* [SSPR 中的所有选项有哪些？它们有哪些含义？](concept-sspr-howitworks.md)
* [我认为有些功能被破坏。如何对 SSPR 进行故障排除？](active-directory-passwords-troubleshoot.md)
* [我有在别处未涵盖的问题](active-directory-passwords-faq.md)

[Contact]: ./media/concept-sspr-customization/sspr-contact-admin.png "联系管理员请求帮忙重置密码的电子邮件示例"
