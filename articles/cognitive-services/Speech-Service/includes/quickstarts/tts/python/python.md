---
title: 快速入门：合成语音，Python - 语音服务
titleSuffix: Azure Cognitive Services
description: 了解如何在 Python 中使用语音 SDK 合成语音
services: cognitive-services
author: trevorbye
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: include
origin.date: 04/04/2020
ms.date: 04/20/2020
ms.author: v-tawe
ms.openlocfilehash: 2d220761ff327150cded52726070b158d8d11e17
ms.sourcegitcommit: f9c242ce5df12e1cd85471adae52530c4de4c7d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/24/2020
ms.locfileid: "82150768"
---
## <a name="prerequisites"></a>先决条件

在开始之前，请务必：

> [!div class="checklist"]
> * [创建一个 Azure 搜索资源](../../../../get-started.md)
> * [设置开发环境并创建空项目](../../../../quickstarts/setup-platform.md?pivots=programming-language-python)
> * 需要语音 SDK 版本 1.10.0 或更高版本。

## <a name="support-and-updates"></a>支持和更新

语音 SDK Python 包的更新将通过 PyPI 分发，[发行说明](~/articles/cognitive-services/Speech-Service/releasenotes.md)中会发布相关通告。
如果有新版本可用，可以使用 `pip install --upgrade azure-cognitiveservices-speech` 命令进行更新。
通过查看 `azure.cognitiveservices.speech.__version__` 变量来检查当前安装的版本。

如果遇到问题或者缺少某项功能，请查看[支持和帮助选项](~/articles/cognitive-services/Speech-Service/support.md)。

## <a name="create-a-python-application-that-uses-the-speech-sdk"></a>使用语音 SDK 创建 Python 应用程序

### <a name="run-the-sample"></a>运行示例

可将本快速入门中的[示例代码](#sample-code)复制到源文件 `quickstart.py`，然后在 IDE 或控制台中运行该代码

```sh
python quickstart.py
```

或者，可以从[语音 SDK 示例存储库](https://github.com/Azure-Samples/cognitive-services-speech-sdk)以 [Jupyter](https://jupyter.org) Notebook 的形式下载本快速入门教程，并将其作为 Notebook 运行。

### <a name="sample-code"></a>代码示例

```python
import azure.cognitiveservices.speech as speechsdk

# Creates an instance of a speech config with specified subscription key and service region.
# Replace with your own subscription key and service region (e.g., "chinaeast2").
speech_host, speech_key = "https://YourServiceRegion.tts.speech.azure.cn/", "YourSubscriptionKey"
speech_config = speechsdk.SpeechConfig(host=speech_host, subscription=speech_key)

# Creates a speech synthesizer using the default speaker as audio output.
speech_synthesizer = speechsdk.SpeechSynthesizer(speech_config=speech_config)

# Receives a text from console input.
print("Type some text that you want to speak...")
text = input()

# Synthesizes the received text to speech.
# The synthesized speech is expected to be heard on the speaker with this line executed.
result = speech_synthesizer.speak_text_async(text).get()

# Checks result.
if result.reason == speechsdk.ResultReason.SynthesizingAudioCompleted:
    print("Speech synthesized to speaker for text [{}]".format(text))
elif result.reason == speechsdk.ResultReason.Canceled:
    cancellation_details = result.cancellation_details
    print("Speech synthesis canceled: {}".format(cancellation_details.reason))
    if cancellation_details.reason == speechsdk.CancellationReason.Error:
        if cancellation_details.error_details:
            print("Error details: {}".format(cancellation_details.error_details))
    print("Did you update the subscription info?")
```

### <a name="install-and-use-the-speech-sdk-with-visual-studio-code"></a>通过 Visual Studio Code 安装并使用语音 SDK

1. 在计算机上下载并安装 64 位版本的 [Python](https://www.python.org/downloads/)（3.5 到 3.8）。
1. 下载并安装 [Visual Studio Code](https://code.visualstudio.com/Download)。
1. 打开 Visual Studio Code 并安装 Python 扩展。 在菜单中选择“文件”   >   “首选项” >   “扩展”。 搜索 **Python**。

   ![安装 Python 扩展](~/articles/cognitive-services/Speech-Service/media/sdk/qs-python-vscode-python-extension.png)

1. 创建一个文件夹用于存储项目。 例如，使用 Windows 资源管理器。
1. 在 Visual Studio Code 中选择“文件”图标。  然后打开创建的文件夹。

   ![打开文件夹](~/articles/cognitive-services/Speech-Service/media/sdk/qs-python-vscode-python-open-folder.png)

1. 选择“新建文件”图标创建新的 Python 源文件 `speechsdk.py`。

   ![创建文件](~/articles/cognitive-services/Speech-Service/media/sdk/qs-python-vscode-python-newfile.png)

1. 复制 [Python 代码](#sample-code)并将其粘贴到新建的文件，然后保存文件。
1. 插入语音服务订阅信息。
1. 如果已选择 Python 解释器，窗口底部的状态栏左侧会显示它。
   否则，会显示可用 Python 解释器的列表。 打开命令面板 (Ctrl+Shift+P) 并输入 **Python:Select Interpreter**。 选择适当的解释器。
1. 如果尚未为所选的 Python 解释器安装， 可以从 Visual Studio Code 内部安装语音 SDK Python 包。
   若要安装语音 SDK 包，请打开终端。 再次启动命令面板 (Ctrl+Shift+P) 并输入 **Terminal:Create New Integrated Terminal** 来打开终端。
   在打开的终端中，输入命令 `python -m pip install azure-cognitiveservices-speech`，或者输入适用于系统的命令。
1. 若要运行示例代码，请在编辑器中的某个位置单击右键。 选择“在终端中运行 Python 文件”。 
   在出现提示时键入一些文本。 合成的音频稍后播放。

   ![运行示例](~/articles/cognitive-services/Speech-Service/media/sdk/qs-python-vscode-python-run-tts.png)

如果在遵照这些说明操作时遇到问题，请参阅内容更全面的 [Visual Studio Code Python 教程](https://code.visualstudio.com/docs/python/python-tutorial)。

## <a name="next-steps"></a>后续步骤

[!INCLUDE [Speech synthesis basics](../../text-to-speech-next-steps.md)]

<!-- ## See also -->

<!-- - [Create a Custom Voice](~/articles/cognitive-services/Speech-Service/how-to-custom-voice-create-voice.md) -->
<!-- - [Record custom voice samples](~/articles/cognitive-services/Speech-Service/record-custom-voice-samples.md) -->
