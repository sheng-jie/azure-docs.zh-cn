---
title: 什么是 Azure 语音服务？
titleSuffix: Azure Cognitive Services
description: Azure 语音服务在单个 Azure 订阅中统合了语音转文本、文本转语音以及语音翻译功能。 使用语音 SDK、语音设备 SDK 或 REST API 可以轻松在应用程序、工具和设备中添加语音。 可将语音功能添加到现有的聊天机器人，在翻译应用程序中将文本转换为语音，或者听录大量的呼叫中心数据。
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: overview
ms.date: 03/13/2019
ms.author: erhopf
ms.openlocfilehash: d4587b5268635691d55b51a7bf88bbe01df2a0c4
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/19/2019
ms.locfileid: "57871631"
---
# <a name="what-are-the-speech-services"></a>什么是语音服务？

Azure 语音服务在单个 Azure 订阅中统合了语音转文本、文本转语音以及语音翻译功能。 使用[语音 SDK](speech-sdk-reference.md)、[语音设备 SDK](speech-devices-sdk-qsg.md) 或 [REST API](rest-apis.md) 可以轻松在应用程序、工具和设备中启用语音。

> [!IMPORTANT]
> 语音服务已替代必应语音 API、语音翻译和自定义语音。 有关迁移说明，请参阅*操作指南 > 迁移*。

这些功能构成了 Azure 语音服务。 请使用下表中的链接详细了解每项功能的常见用例或浏览 API 参考信息。

| 服务 | Feature | 说明 | SDK 中 IsInRole 中的声明 | REST |
|---------|---------|-------------|-----|------|
| [语音转文本](speech-to-text.md) | 语音转文本 | 语音转文本可将音频流实时听录为应用程序、工具或设备可以使用或显示的文本。 结合[语言理解 (LUIS)](https://docs.microsoft.com/azure/cognitive-services/luis/) 使用语音转文本可以从听录的语音中派生用户意向，以及处理语音命令。 | [是](https://docs.microsoft.com/azure/cognitive-services/speech-service/speech-sdk-reference) | [是](https://docs.microsoft.com/azure/cognitive-services/speech-service/rest-apis) |
| | [批量听录](batch-transcription.md) | 使用批量听录能够以异步方式对大量的数据进行语音转文本听录。 这是一个基于 REST 的服务，它使用的终结点与自定义和模型管理相同。 | 否 | [是](https://westus.cris.ai/swagger/ui/index) |
| | [自定义](#customize-your-speech-experience) | 如果使用语音转文本在独特的环境中进行识别和听录，则可以创建并训练自定义的声学、语言和发音模型，以解决环境干扰或行业特定的词汇。 | 否 | [是](https://westus.cris.ai/swagger/ui/index) |
| [文本转语音](text-to-speech.md) | 文本转语音 | 文本转语音可将输入文本转换为类似人类的合成语音。 可以选择标准语音或神经语音（请参阅[语言支持](language-support.md)）。 | 否 | [是](https://docs.microsoft.com/azure/cognitive-services/speech-service/rest-apis) |
| | [自定义](#customize-your-speech-experience) | 创建专属于品牌或产品的自定义语音字体。 | 否 | [是](https://docs.microsoft.com/azure/cognitive-services/speech-service/rest-apis) |
| [语音翻译](speech-translation.md) | 语音翻译 | 使用语音翻译可在应用程序、工具和设备中实现实时的多语言语音翻译。 进行语音转语音和语音转文本翻译时可以使用此服务。 | [是](https://docs.microsoft.com/azure/cognitive-services/speech-service/speech-sdk-reference) | 否 |

## <a name="news-and-updates"></a>新增功能和更新

了解 Azure 语音服务的新增功能。

* 2019 年 2 月 - 发布了支持 [Unity (beta)](quickstart-csharp-unity.md) 的语音 SDK 1.3.0。 添加了对 `AudioInput` 类的支持，使用该类可以选择音频流源。 有关增强功能和已知问题的完整列表，请参阅[发行说明](releasenotes.md)。
* 2018 年 12 月 - 发布了支持 [Python](quickstart-python.md)、[Node.js](quickstart-js-node.md) 和 Ubuntu 18.04 LTS 的语音 SDK 1.2.0。 有关详细信息，请参阅[发行说明](releasenotes.md)。
* 2018 年 12 月 - 添加了适用于 [.NET Core](quickstart-dotnet-text-to-speech.md)、[Python](quickstart-python-text-to-speech.md) 和 [Node.js](quickstart-nodejs-text-to-speech.md) 的文本转语音快速入门。 [GitHub](https://github.com/Azure-Samples/Cognitive-Speech-TTS/tree/master/Samples-Http) 中提供了更多示例。

## <a name="try-speech-services"></a>试用语音服务

我们提供了适用于大多数流行编程语言的快速入门，旨在帮助你在 10 分钟以内运行代码。 下表包含有关每项功能在最流行编程语言中的用法的快速入门。 使用左侧的导航栏可以浏览其他语言和平台。

| 语音转文本 (SDK) | 翻译 (SDK) | 文本转语音 (REST) |
|-------------------|-------------------|-----------------------|
| [C#、.NET Core (Windows)](quickstart-csharp-dotnet-windows.md) | [Java（Windows、Linux）](quickstart-translate-speech-java-jre.md) | [Python（Windows、Linux、macOS）](quickstart-python-text-to-speech.md) |
| [Javascript（浏览器）](quickstart-js-browser.md) | [C#、.NET Core (Windows)](quickstart-translate-speech-dotnetcore-windows.md) | [C#、.NET Core（Windows、Linux、macOS）](quickstart-dotnet-text-to-speech.md) |
| [Python（Windows、Linux、macOS）](quickstart-python.md) | [C#、.NET Framework (Windows)](quickstart-translate-speech-dotnetframework-windows.md) | [Node.js（Windows、Linux、macOS）](quickstart-nodejs-text-to-speech.md) |
| [Java（Windows、Linux）](quickstart-java-jre.md) | [C++ (Windows)](quickstart-translate-speech-cpp-windows.md) | |

有机会使用语音服务后，请尝试学习有关如何使用语音 SDK 和 LUIS 从语音中识别意向的教程。

* [教程：使用适用于 C# 的语音 SDK 和 LUIS 从语音中识别意向](how-to-recognize-intents-from-speech-csharp.md)

## <a name="get-sample-code"></a>获取示例代码

GitHub 中提供了每个 Azure 语音服务的示例代码。 这些示例涵盖了常见方案，例如，从文件或流中读取音频、连续和单次识别，以及使用自定义模型。 使用以下链接查看 SDK 和 REST 示例：

* [语音转文本和语音翻译示例 (SDK)](https://github.com/Azure-Samples/cognitive-services-speech-sdk)
* [批量听录示例 (REST)](https://github.com/Azure-Samples/cognitive-services-speech-sdk/tree/master/samples/batch)
* [文本转语音示例 (REST)](https://github.com/Azure-Samples/Cognitive-Speech-TTS)

## <a name="customize-your-speech-experience"></a>自定义语音体验

Azure 语音服务能够很好地与内置模型配合工作，但是，你可能想要根据自己的产品或环境，进一步自定义和优化体验。 自定义选项的范围从声学模型优化，到专属于自有品牌的语音字体。 生成自定义模型后，可将其与任何 Azure 语音服务配合使用。

| 语音服务 | 模型 | 说明 |
|----------------|-------|-------------|
| 语音转文本 | [声学模型](how-to-customize-acoustic-models.md) | 为特定环境（例如汽车或工厂车间）中使用的应用程序、工具或设备创建自定义声学模型，每个模型具有特定的录制条件。 示例包括带有口音的讲话、特定的背景噪音，或使用特定的麦克风录制音频。 |
| | [语言模型](how-to-customize-language-model.md) | 创建自定义语言模型来改善特定领域的词汇和语法的听录，例如医疗术语中或 IT 行话。 |
| | [发音模型](how-to-customize-pronunciation.md) | 借助自定义发音模型，可以定义语音形式以及字词或术语的显示。 它适用于处理自定义术语，如产品名称或首字母缩略词。 只需使用发音文件（简单的 .txt 文件）即可。 |
| 文本转语音 | [语音字体](how-to-customize-voice-font.md) | 使用自定义语音字体可为自有品牌创建可识别的独一无二的声音。 只需使用少量的数据即可开始创建。 提供的数据越多，语音字体就越自然，且越接近人类语音。 |

## <a name="reference-docs"></a>参考文档

* [语音 SDK](speech-sdk-reference.md)
* [语音设备 SDK](speech-devices-sdk.md)
* [REST API：语音转文本](rest-speech-to-text.md)
* [REST API：文本转语音](rest-text-to-speech.md)
* [REST API：批量听录和自定义](https://westus.cris.ai/swagger/ui/index)

## <a name="next-steps"></a>后续步骤

> [!div class="nextstepaction"]
> [免费获取语音服务订阅密钥](get-started.md)
