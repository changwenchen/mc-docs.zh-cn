---
title: 关于语音 SDK - 语音服务
titleSuffix: Azure Cognitive Services
description: 语音软件开发工具包 (SDK) 公开了许多语音服务功能，这使得开发支持语音的应用程序更为容易。
services: cognitive-services
author: trevorbye
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
origin.date: 04/02/2020
ms.date: 04/20/2020
ms.author: v-tawe
ms.openlocfilehash: b3a3e10e483d9dec2a967ad0dc28d9d5a1056c20
ms.sourcegitcommit: a4a2521da9b29714aa6b511fc6ba48279b5777c8
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/24/2020
ms.locfileid: "82126960"
---
# <a name="about-the-speech-sdk"></a>关于语音 SDK

语音软件开发工具包 (SDK) 公开了许多语音服务功能，这使得你能够开发支持语音的应用程序。 语音 SDK 可以在许多编程语言中和所有平台中使用。

[!INCLUDE [Speech SDK Platforms](../../../includes/cognitive-services-speech-service-speech-sdk-platforms.md)]

## <a name="scenario-capabilities"></a>方案功能

语音 SDK 公开了语音服务中的许多功能，但未公开全部功能。 语音 SDK 的功能通常与方案相关联。 语音 SDK 同时适用于实时和非实时方案，使用本地设备、文件、Azure Blob 存储甚至输入和输出流。 如果无法通过语音 SDK 实现某个方案，请寻求使用 REST API 替代方法。

### <a name="speech-to-text"></a>语音转文本

[语音转文本](speech-to-text.md)（也称为“语音识别”  ）可将音频流听录为应用程序、工具或设备可以使用或显示的文本。 结合[语言理解 (LUIS)](../luis/index.yml) 使用语音转文本可以从听录的语音中派生用户意向，以及处理语音命令。 使用[语音翻译](speech-translation.md)通过单个调用将语音输入翻译为另一种语言。 有关详细信息，请参阅[语音转文本基础知识](speech-to-text-basics.md)。

### <a name="text-to-speech"></a>文本转语音

[文本转语音](text-to-speech.md)（也称为“语音合成”  ）将文本转换为类似人类语言的合成语音。 输入文本是字符串文字或使用[语音合成标记语言 (SSML)](speech-synthesis-markup.md)。 有关标准语音或神经语音的详细信息，请参阅[文本转语音语言和语音支持](language-support.md#text-to-speech)。

<!-- ### Voice assistants -->

<!-- #### Keyword spotting -->

<!-- ### Meeting scenarios -->

<!-- #### Conversation Transcription -->

<!-- #### Multi-device Conversation -->

<!-- ### Custom / agent scenarios -->

<!-- #### Call Center Transcription -->

### <a name="codec-compressed-audio-input"></a>编解码器压缩的音频输入

一些语音 SDK 编程语言支持编解码器压缩的音频输入流。 有关详细信息，请参阅<a href="https://docs.azure.cn/cognitive-services/speech-service/how-to-use-codec-compressed-audio-input-streams" target="_blank">使用压缩的音频输入格式 <span class="docon docon-navigate-external x-hidden-focus"></span></a>。

## <a name="rest-api"></a>REST API

虽然语音 SDK 涵盖了语音服务的许多功能，但对于某些方案，你可能需要使用 REST API。

### <a name="batch-transcription"></a>批量听录

使用[批量听录](batch-transcription.md)能够以异步方式对大量的数据进行语音转文本听录。 只能通过 REST API 使用批量听录。 除了将语音音频转换为文本，批量语音转文本还允许进行分割聚类和情感分析。

## <a name="customization"></a>自定义

语音服务在语音转文本、文本转语音和语音翻译方面提供了强大的功能和默认模型。 有时，你可能希望提高基线性能，以便更好地处理你的独特用例。 语音服务有各种各样的无代码自定义工具，这些工具使上述事项变得简单，并使你能够使用基于你自己的数据的自定义模型获得竞争优势。 这些模型将仅供你和你的组织使用。

### <a name="custom-speech-to-text"></a>自定义语音转文本

使用语音转文本在独特的环境中进行识别和听录时，可以创建并训练自定义的声学、语言和发音模型，以解决环境干扰或行业特定的词汇的问题。 可通过[自定义语音识别门户](https://aka.ms/customspeech)来创建和管理无代码自定义语音识别模型。 自定义语音识别模型在发布后可以由语音 SDK 使用。

<!-- ### Custom Text-to-speech -->

## <a name="get-the-speech-sdk"></a>获取语音 SDK

> [!IMPORTANT]
> 需要语音 SDK 1.10.0 或更高版本。

# <a name="windows"></a>[Windows](#tab/windows)

[!INCLUDE [Get the Speech SDK](includes/get-speech-sdk-windows.md)]

# <a name="linux"></a>[Linux](#tab/linux)

[!INCLUDE [Get the Speech SDK](includes/get-speech-sdk-linux.md)]

# <a name="ios"></a>[iOS](#tab/ios)

[!INCLUDE [Get the Speech SDK](includes/get-speech-sdk-ios.md)]

# <a name="macos"></a>[macOS](#tab/macos)

[!INCLUDE [Get the Speech SDK](includes/get-speech-sdk-macos.md)]

# <a name="android"></a>[Android](#tab/android)

[!INCLUDE [Get the Speech SDK](includes/get-speech-sdk-android.md)]

# <a name="nodejs"></a>[Node.js](#tab/nodejs)

[!INCLUDE [Get the Node.js Speech SDK](includes/get-speech-sdk-nodejs.md)]

# <a name="browser"></a>[浏览器](#tab/browser)

[!INCLUDE [Get the Browser Speech SDK](includes/get-speech-sdk-browser.md)]

---

[!INCLUDE [License notice](../../../includes/cognitive-services-speech-service-license-notice.md)]

[!INCLUDE [Sample source code](../../../includes/cognitive-services-speech-service-speech-sdk-sample-download-h2.md)]

## <a name="next-steps"></a>后续步骤

* [获取语音试用订阅](https://www.azure.cn/home/features/cognitive-services/)
* [了解如何在 C# 中识别语音](quickstarts/speech-to-text-from-microphone.md?pivots=programming-language-csharp&tabs=dotnet)
