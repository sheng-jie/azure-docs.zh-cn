---
title: Azure 媒体服务概念-Azure |Microsoft Docs
description: 本主题简要概述了 Azure 媒体服务概念，并提供有关详细信息的链接。
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: article
ms.date: 03/20/2019
ms.author: juliako
ms.custom: seodec18
ms.openlocfilehash: d8790eac93b288d5d5254f188fe5c901b5d5df14
ms.sourcegitcommit: 87bd7bf35c469f84d6ca6599ac3f5ea5545159c9
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/22/2019
ms.locfileid: "58351484"
---
# <a name="media-services-concepts"></a>媒体服务概念

本主题简要概述了 Azure 媒体服务概念，并使用媒体服务 v3 概念和功能的深入说明提供文章的链接。 在开始开发之前，应该复习这些主题中所述的基本概念。

> [!NOTE]
> 目前，无法使用 Azure 门户来管理 v3 资源。 使用[REST API](https://aka.ms/ams-v3-rest-ref)， [CLI](https://aka.ms/ams-v3-cli-ref)，或某个受支持[Sdk](developers-guide.md)。

## <a name="cloud-upload-and-storage"></a>云上传和存储

若要开始管理、 加密、 编码、 分析，以及在 Azure 中的媒体内容的流式处理，您需要创建媒体服务帐户并上传到数字文件**资产**。

- [云上传和存储](storage-account-concept.md)
- [资产概念](assets-concept.md)

## <a name="encoding"></a>编码

一旦将高质量的数字媒体文件上传到资产，可以对它们进行编码为可在各种浏览器和设备播放的格式。 

若要使用媒体服务 v3 进行编码，您需要创建**转换**并**作业**。

![转换](./media/encoding/transforms-jobs.png)

- [转换和作业](transforms-jobs-concept.md)
- [使用媒体服务编码](encoding-concept.md)

## <a name="media-analytics"></a>媒体分析

若要分析你的视频和音频文件，还需要创建**转换**并**作业**。

- [分析视频和音频文件](analyzing-video-audio-files-concept.md)

## <a name="packaging-delivery-protection"></a>打包、传送、保护

一旦你的内容进行编码时，可以充分利用**动态打包**。 **流式处理终结点**是用于向客户端播放器提供媒体内容的媒体服务中的动态打包服务。 若要使视频输出资产中播放的客户端，您必须创建**流式处理定位符**，然后生成流式处理 Url。 

创建时**流式处理定位符**，需要指定资产的名称，除了**流式处理策略**。 **流式处理策略**使您能够定义流式处理协议和加密选项 （如果有） 为你**流式处理定位符**。

流式传输实时或按需内容是否使用动态打包。 下图显示了按需流式处理与动态打包工作流。

![动态打包](./media/dynamic-packaging-overview/media-services-dynamic-packaging.svg)

借助媒体服务中，可以传送使用高级加密标准 (AES-128) 动态加密的实时和按需内容或 / 和任何三个主要数字版权管理 (DRM) 系统：内容。 媒体服务还提供了用于向已授权客户端传送 AES 密钥和 DRM（PlayReady、Widevine 和 FairPlay）许可证的服务。

如果指定流上的加密选项，创建**内容密钥策略**并将其与你**流式处理定位符**。 **内容密钥策略**使你能够配置内容密钥传递的方式来结束客户端。

下图阐释了媒体服务内容保护工作流： 

![保护内容](./media/content-protection/content-protection.svg)

&#42;动态加密支持 AES-128"明文密钥"，CBC 和 CENC。 

可以使用媒体服务**动态清单**仅特定再现内容或子剪辑的视频流式传输。 以下示例使用编码器将夹层资产编码成七个 ISO MP4 视频再现内容（从 180p 到 1080p）。 编码的资产可以动态打包成以下任一流式处理协议：HLS、MPEG DASH 和平滑流式处理。  图表顶部显示了不包含筛选器的资产的 HLS 清单（包含全部七个再现内容）。  左下角显示名为“ott”的筛选器已应用到 HLS 清单。 “ott”筛选器指定要删除所有不低于 1 Mbps 的比特率，因此将最差的两个质量级别从响应中剥除。 在右下角显示已应用名为“mobile”的筛选器的 HLS 清单。 “mobile”筛选器指定删除分辨率大于 720p 的再现内容，因此会剥除两个 1080p 再现内容。

![再现内容筛选](./media/filters-dynamic-manifest-overview/media-services-rendition-filter.png)

- [动态打包](dynamic-packaging-overview.md)
- [流式处理终结点](streaming-endpoint-concept.md)
- [流式处理定位符](streaming-locators-concept.md)
- [流式处理策略](streaming-policy-concept.md)
- [内容密钥策略](content-key-policy-concept.md)
- [内容保护](content-protection-overview.md)
- [动态清单](filters-dynamic-manifest-overview.md)
- [筛选器](filters-concept.md)

## <a name="live-streaming"></a>实时传送视频流

使用 Azure 媒体服务可将实时事件传送到 Azure 云中的客户。 **实时事件**负责引入和处理实时视频源。 在创建时**Live 事件**，可以使用从远程编码器将实时信号创建输入终结点。 将流传输到后**Live 事件**，可以通过创建启动流式处理事件**资产**， **Live 输出**，和**流式处理定位符**. **Live 输出**存档到流**资产**并使其可通过观看者**流式处理终结点**。 一个**Live 事件**可以是两种类型之一：**直通**并**实时编码**。

下图阐释了传递类型工作流：

![直通](./media/live-streaming/pass-through.svg)

- [实时传送视频流概述](live-streaming-overview.md)
- [实时事件和实时输出](live-events-outputs-concept.md)

## <a name="monitoring"></a>监视

### <a name="event-grid"></a>事件网格

若要查看作业的进度，应使用**事件网格**。 媒体服务还会发出实时事件类型。 使用事件网格，应用可以侦听和响应来自几乎所有 Azure 服务和自定义源的事件。 

- [处理事件网格事件](reacting-to-media-services-events.md)
- [架构](media-services-event-schemas.md)
- [指标和诊断日志](media-services-metrics-diagnostic-logs.md)
- [诊断日志架构](media-services-diagnostic-logs-schema.md)

## <a name="player-clients"></a>播放器客户端

可以使用 Azure 媒体播放器播放媒体内容由在各种浏览器和设备上的媒体服务流式处理。 Azure 媒体播放器采用行业标准（如 HTML5、媒体源扩展 (MSE) 和加密媒体扩展插件 (EME)）来提供更丰富的自适应流式处理体验。 

- [Azure Media Player 概述](use-azure-media-player.md)

## <a name="next-steps"></a>后续步骤

[使用媒体服务上传、编码和流式传输](stream-files-tutorial-with-api.md)
