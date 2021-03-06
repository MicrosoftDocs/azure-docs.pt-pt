---
title: Streaming codec comprimido áudio com o Serviço de Discurso SDK - Speech
titleSuffix: Azure Cognitive Services
description: Aprenda a transmitir áudio comprimido para o serviço de Discurso com o SDK do discurso. Disponível para C++, C#e Java para Linux, Java no Android e Objective-C no iOS.
services: cognitive-services
author: amitkumarshukla
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 03/30/2020
ms.author: amishu
ms.custom: devx-track-csharp
zone_pivot_groups: programming-languages-set-twenty-two
ms.openlocfilehash: db902019b4fb1237c8403c719862d8fca4ba4f28
ms.sourcegitcommit: 4b0e424f5aa8a11daf0eec32456854542a2f5df0
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/20/2021
ms.locfileid: "107772528"
---
# <a name="use-codec-compressed-audio-input-with-the-speech-sdk"></a>Utilize entrada de áudio comprimido codec com o SDK de fala

O serviço de voz SDK pode aceitar formatos de áudio comprimidos. Descoomente o áudio antes de enviá-lo por cima do fio para o serviço de fala como PCM cru.

Plataforma | Idiomas | Versão GStreamer suportada
| :--- | ---: | :---:
Janelas (excluindo UWP)  | C++, C#, Java, Python | [1.18.3](https://gstreamer.freedesktop.org/data/pkg/windows/1.18.3/)
Linux  | C++, C#, Java, Python | [apoiou distribuição de Linux e arquiteturas-alvo](~/articles/cognitive-services/speech-service/speech-sdk.md)
Android  | Java | [1.18.3](https://gstreamer.freedesktop.org/data/pkg/android/1.18.3/)

## <a name="speech-sdk-version-required-for-compressed-audio-input"></a>Versão SDK de fala necessária para entrada de áudio comprimido
* A versão SDK de fala 1.10.0 ou posterior é necessária para RHEL 8 e CentOS 8
* A versão SDK do discurso 1.11.0 ou posterior é necessária para o Windows.
* Versão SDK do discurso 1.16.0 ou mais tarde para o gstreamer mais recente no Windows e Android.

[!INCLUDE [supported-audio-formats](includes/supported-audio-formats.md)]

## <a name="gstreamer-required-to-handle-compressed-audio"></a>GStreamer necessário para manusear áudio comprimido

::: zone pivot="programming-language-csharp"
[!INCLUDE [prerequisites](includes/how-to/compressed-audio-input/csharp/prerequisites.md)]
::: zone-end

::: zone pivot="programming-language-cpp"
[!INCLUDE [prerequisites](includes/how-to/compressed-audio-input/cpp/prerequisites.md)]
::: zone-end

::: zone pivot="programming-language-java"
[!INCLUDE [prerequisites](includes/how-to/compressed-audio-input/java/prerequisites.md)]
::: zone-end

::: zone pivot="programming-language-python"
[!INCLUDE [prerequisites](includes/how-to/compressed-audio-input/python/prerequisites.md)]
::: zone-end

## <a name="example-code-using-codec-compressed-audio-input"></a>Código de exemplo usando entrada de áudio comprimido codec

::: zone pivot="programming-language-csharp"
[!INCLUDE [prerequisites](includes/how-to/compressed-audio-input/csharp/examples.md)]
::: zone-end

::: zone pivot="programming-language-cpp"
[!INCLUDE [prerequisites](includes/how-to/compressed-audio-input/cpp/examples.md)]
::: zone-end

::: zone pivot="programming-language-java"
[!INCLUDE [prerequisites](includes/how-to/compressed-audio-input/java/examples.md)]
::: zone-end

::: zone pivot="programming-language-python"
[!INCLUDE [prerequisites](includes/how-to/compressed-audio-input/python/examples.md)]
::: zone-end

## <a name="next-steps"></a>Passos seguintes

> [!div class="nextstepaction"]
> [Aprenda a reconhecer a fala](./get-started-speech-to-text.md)
