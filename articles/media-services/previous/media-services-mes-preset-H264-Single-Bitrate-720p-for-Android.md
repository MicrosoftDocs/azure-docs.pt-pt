---
title: H264 Single Bitrate 720p para Android | Microsoft Docs
description: O tópico apresenta uma visão geral do **Bitrate Single Bitrate 720p H264 para** a pré-sintonia de tarefas android.
author: IngridAtMicrosoft
manager: femila
editor: ''
services: media-services
documentationcenter: ''
ms.assetid: 4f9569a3-5aca-4fea-8242-024925a8af90
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/10/2021
ms.author: inhenkel
ms.openlocfilehash: a05203925c0e731c1c05e0c97d0f67f997419970
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/30/2021
ms.locfileid: "103016445"
---
# <a name="h264-single-bitrate-720p-for-android"></a>H264 Taxa de Bits Única 720p para Android

[!INCLUDE [media services api v2 logo](./includes/v2-hr.md)]

`Media Encoder Standard` define um conjunto de predefinições codificantes que pode usar ao criar trabalhos de codificação. Pode utilizar um `preset name` para especificar em que formato gostaria de codificar o seu ficheiro de mídia. Ou, pode criar as suas próprias predefinições baseadas em JSON ou XML (utilizando codificação UTF-8 ou UTF-16. Passaria então a predefinição personalizada para o codificar. Para a lista de todos os nomes predefinidos suportados por este `Media Encoder Standard` codificadores, consulte [Task Presets for Media Encoder Standard](media-services-mes-presets-overview.md).  
  
Este tópico mostra a `H264 Single Bitrate 720p for Android` predefinição no formato XML e JSON.  
  
Esta predefinição produz um único ficheiro MP4 com um bitrate de 2000 kbps, e estéreo AAC. Para obter informações detalhadas sobre o perfil, o bitrate, a taxa de amostragem, etc. desta predefinição, examine o XML ou JSON definido abaixo. Para obter explicações sobre o que significa cada elemento nestas predefinições e os valores válidos para cada elemento, consulte o tópico [de esquema padrão Media Encoder.](media-services-mes-schema.md)  
  
 XML  
  
```
<?xml version="1.0" encoding="utf-16"?>
<Preset xmlns:xsd="https://www.w3.org/2001/XMLSchema" xmlns:xsi="https://www.w3.org/2001/XMLSchema-instance" Version="1.0" xmlns="https://www.windowsazure.com/media/encoding/Preset/2014/03">
  <Encoding>
    <H264Video>
      <KeyFrameInterval>00:00:05</KeyFrameInterval>
      <SceneChangeDetection>true</SceneChangeDetection>
      <H264Layers>
        <H264Layer>
          <Bitrate>2000</Bitrate>
          <Width>1280</Width>
          <Height>720</Height>
          <FrameRate>0/1</FrameRate>
          <Profile>Baseline</Profile>
          <Level>3.1</Level>
          <BFrames>0</BFrames>
          <ReferenceFrames>3</ReferenceFrames>
          <Slices>0</Slices>
          <AdaptiveBFrame>false</AdaptiveBFrame>
          <EntropyMode>Cavlc</EntropyMode>
          <BufferWindow>00:00:05</BufferWindow>
          <MaxBitrate>2000</MaxBitrate>
        </H264Layer>
      </H264Layers>
      <Chapters />
    </H264Video>
    <AACAudio>
      <Profile>AACLC</Profile>
      <Channels>2</Channels>
      <SamplingRate>48000</SamplingRate>
      <Bitrate>192</Bitrate>
    </AACAudio>
  </Encoding>
  <Outputs>
    <Output FileName="{Basename}_{Width}x{Height}_{VideoBitrate}.mp4">
      <MP4Format />
    </Output>
  </Outputs>
</Preset>
```
  
 JSON  
  
```
{
  "Version": 1.0,
  "Codecs": [
    {
      "KeyFrameInterval": "00:00:05",
      "SceneChangeDetection": true,
      "H264Layers": [
        {
          "Profile": "Baseline",
          "Level": "3.1",
          "Bitrate": 2000,
          "MaxBitrate": 2000,
          "BufferWindow": "00:00:05",
          "Width": 1280,
          "Height": 720,
          "ReferenceFrames": 3,
          "EntropyMode": "Cavlc",
          "Type": "H264Layer",
          "FrameRate": "0/1"
        }
      ],
      "Type": "H264Video"
    },
    {
      "Profile": "AACLC",
      "Channels": 2,
      "SamplingRate": 48000,
      "Bitrate": 192,
      "Type": "AACAudio"
    }
  ],
  "Outputs": [
    {
      "FileName": "{Basename}_{Width}x{Height}_{VideoBitrate}.mp4",
      "Format": {
        "Type": "MP4Format"
      }
    }
  ]
}
```
