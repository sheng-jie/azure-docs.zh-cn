---
title: 在 Microsoft Flow 中使用 Azure IoT Central 连接器生成工作流 | Microsoft Docs
description: 在 Microsoft Flow 中使用 IoT Central 连接器触发工作流并在工作流中创建、更新和删除设备。
services: iot-central
author: viv-liu
ms.author: viviali
ms.date: 02/20/2019
ms.topic: conceptual
ms.service: iot-central
manager: peterpr
ms.openlocfilehash: 555fe54174c9e13319af676cab3a5d3dcfaf2fe5
ms.sourcegitcommit: 5fbca3354f47d936e46582e76ff49b77a989f299
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/12/2019
ms.locfileid: "57770243"
---
# <a name="build-workflows-with-the-iot-central-connector-in-microsoft-flow"></a>在 Microsoft Flow 中使用 IoT Central 连接器生成工作流

本主题适用于构建者和管理员。

使用 Microsoft Flow 跨企业用户依赖的多个应用程序和服务自动完成工作流。 如果在 Microsoft Flow 中使用 IoT Central 连接器，则当规则在 IoT Central 中触发时，就可以触发工作流。 在 IoT Central 或任何其他应用程序触发的工作流中，可以使用 IoT Central 连接器中的操作来创建设备、更新设备的属性和设置，或者删除设备。 请检查[这些 Microsoft Flow 模板](https://aka.ms/iotcentralflowtemplates)，此类模板可以将 IoT Central 连接到其他服务，例如移动通知和 Microsoft Teams。

## <a name="prerequisites"></a>必备组件

- 即用即付应用程序
- 用于登录 Flow 的 Microsoft 个人、工作或学校帐户（[了解有关 Microsoft Flow 的详细信息](https://aka.ms/microsoftflowplans)）

## <a name="trigger-a-workflow"></a>触发工作流

本部分演示了如何触发时 IoT Central 中的规则将触发的 Flow 移动应用中的移动通知。

1. 首先[在 IoT Central 中创建规则](howto-create-telemetry-rules.md)。 保存规则条件后，选择**Microsoft Flow 操作**作为新的操作。 此时会在浏览器中打开新的标签页或窗口，让你进入 Microsoft Flow 中。

    ![新建 Microsoft Flow 操作](media/howto-add-microsoft-flow/createflowaction.png)

1. 登录到 Microsoft Flow。 此帐户不需要是在 IoT Central 中使用的帐户。 此时会登陆到一个概览页，显示 IoT Central 连接器正在连接到自定义操作。

1. 登录到 IoT Central 连接器并选择**继续**。 此时会转到 Microsoft Flow 设计器，可以在其中生成工作流。 工作流有一个 IoT Central 触发器，其中已填充应用程序和规则。

1. 选择“+ 新建步骤”和“添加操作”。 此时可向工作流添加任何所需操作。 例如，可以发送移动通知。 搜索“通知”，然后选择“通知 - 向我发送移动通知”。

1. 在操作的“文本”字段中填充希望通知中包含的内容。 可以包括 IoT Central 规则提供的动态内容，将设备名称和时间戳等重要信息传递给通知。

    > [!NOTE]
    > 选择**查看更多**动态内容的窗口，以获取触发规则的度量值和属性值中的文本。

    ![Flow 编辑操作，此时动态窗格处于打开状态](./media/howto-add-microsoft-flow/flowdynamicpane.png)

1. 完成编辑你的操作，选择**保存**。 此时会定向到工作流的概览页。 在这里可以看到运行历史记录，并可将它与其他同事共享。

    > [!NOTE]
    > 如果希望 IoT Central 应用中的其他用户编辑此规则，则必须在 Microsoft Flow 中将其与这些用户共享。 在工作流中将这些用户的 Microsoft Flow 帐户作为所有者添加。

1. 如果回到 IoT Central 应用，则会看到此规则在“操作”区域下有一个 Microsoft Flow 操作。

随时可以在 Microsoft Flow 中使用 IoT Central 连接器开始生成工作流。 然后可以选择要连接到的具体 IoT Central 应用和规则。

## <a name="create-a-device-in-a-workflow"></a>在工作流中创建设备

此部分介绍如何在移动设备上使用 Microsoft Flow 移动应用通过按钮在 IoT Central 中创建新设备。 可以在 Flow 中使用此操作将 Dynamics 之类的 ERP 系统与 IoT Central 集成，方法是创建一个新设备（如果在其他位置添加了设备）。

1. 一开始在 Microsoft Flow 中创建空白工作流。

1. 搜索作为触发器的“适用于移动设备的流按钮”。

1. 在此触发器中，添加名为“设备名称”的文本输入。 将说明文本更改为“输入新设备的设备名称”。

1. 添加新操作。 搜索“Azure IoT Central - 创建设备”操作。

1. 在下拉列表中选取应用程序，然后选择用于创建设备的设备模板。 此时会看到操作展开，显示设备的所有属性和设置。

1. 选择“设备名称”字段。 在动态内容窗格中，选择“设备名称”。 此值传递从输入用户通过移动应用中，输入，并为新设备在 IoT 中心的名称。 在此示例中，唯一必填字段为设备名称，以红色星号指示。 另一设备模板可能有多个必填字段，需填充这些字段才能创建新设备。

    ![Flow 的创建设备操作动态窗格](./media/howto-add-microsoft-flow/flowcreatedevice.png)

1. （可选）根据需要在其他字段中填充内容，以便创建新设备。

1. 最后，保存工作流。

1. 在 Microsoft Flow 移动应用中试用工作流。 转到应用中的“按钮”选项卡。 此时会看到“按钮 -> 创建新设备”工作流。 输入新设备的名称，观察其在 IoT Central 中显示！

    ![Flow 的创建设备移动屏幕截图](./media/howto-add-microsoft-flow/flowmobilescreenshot.png)

## <a name="update-a-device-in-a-workflow"></a>在工作流中更新设备

此部分介绍如何在移动设备上使用 Microsoft Flow 移动应用通过按钮在 IoT Central 中更新设备设置和属性。

1. 一开始在 Microsoft Flow 中创建空白工作流。

1. 搜索作为触发器的“适用于移动设备的流按钮”。

1. 在此触发器中，请添加一个输入，例如一个与要更改的设置或属性对应的“位置”文本输入。 更改说明文本。

1. 添加新操作。 搜索“Azure IoT Central - 更新设备”操作。

1. 从下拉菜单中选取应用程序。 现在需要 ID，该 ID 属于要更新的现有设备。 可以从 **Device Explorer** 中获得 IoT Central 设备的 ID

    ![IoT Central Device Explorer 设备 ID](./media/howto-add-microsoft-flow/iotcdeviceid.png)

1. 可以更新设备名称。 若要更新设备的属性和设置，必须在“设备模板”下拉列表中选择要更新的设备对应的设备模板。 此时操作磁贴会展开，显示可更新的所有属性和设置。

    ![Flow 的更新设备工作流](./media/howto-add-microsoft-flow/flowupdatedevice.png)

1. 选择要更新的每个属性和设置。 在动态内容窗格的触发器中，选择相应的输入。 在此示例中，“位置”值会向下传播，以便更新设备的“位置”属性。

1. 最后，保存工作流。

1. 在 Microsoft Flow 移动应用中试用工作流。 转到应用中的“按钮”选项卡。 此时会看到“按钮 -> 更新设备”工作流。 输入输入内容，此时会看到设备在 IoT Central 中进行了更新！

## <a name="delete-a-device-in-a-workflow"></a>在工作流中删除设备

可以使用“Azure IoT Central - 删除设备”操作按设备 ID 删除设备。 下面是一个示例工作流，此工作流在 Microsoft Flow 移动应用中通过按钮来删除设备。

   ![Flow 的删除设备工作流](./media/howto-add-microsoft-flow/flowdeletedevice.png)

## <a name="troubleshooting"></a>故障排除

如果无法创建到 Azure IoT Central 连接器的连接，请参考以下提示。

1. Microsoft 个人帐户（例如 @hotmail.com、@live.com、@outlook.com 域）目前不受支持。 你必须使用 Azure Active Directory (AD) 工作或学校帐户。

2. 必须至少已登录到 IoT Central 应用程序一次，才能在 Microsoft Flow 中使用 IoT Central 连接器。 否则此应用程序将无法在“应用程序”下拉菜单中显示。

3. 如果使用的 Azure AD 帐户时收到错误，请尝试打开 Windows PowerShell 并以管理员身份运行以下 commandlet。

    ``` PowerShell
    Install-Module AzureAD
    Connect-AzureAD
    New-AzureADServicePrincipal -AppId 9edfcdd9-0bc5-4bd4-b287-c3afc716aac7 -DisplayName "Azure IoT Central"
    ```

## <a name="next-steps"></a>后续步骤

现在，已了解如何使用 Microsoft Flow 来构建工作流，建议的下一步是向[管理设备](howto-manage-devices.md)。

