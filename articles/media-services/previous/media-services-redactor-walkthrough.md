---
title: Redact enfrenta Azure Media Analytics a percorrer | Microsoft Docs
description: Este tópico mostra instruções passo a passo sobre como executar um fluxo de trabalho de redação completo usando Azure Media Services Explorer (AMSE) e Azure Media Redator Visualizer (ferramenta open source).
services: media-services
documentationcenter: ''
author: IngridAtMicrosoft
manager: femila
editor: ''
ms.assetid: d6fa21b8-d80a-41b7-80c1-ff1761bc68f2
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 03/10/2021
ms.author: ril
ms.reviewer: juliako
ms.openlocfilehash: 1a7e20681dfa7da7ce30f46a7c4b0b6df6f78916
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/30/2021
ms.locfileid: "103009567"
---
# <a name="redact-faces-with-azure-media-analytics-walkthrough"></a>Redact enfrenta com Azure Media Analytics walkthrough

[!INCLUDE [media services api v2 logo](./includes/v2-hr.md)]

## <a name="overview"></a>Descrição Geral

**O Azure Media Redator** é um processador de media [Azure Media Analytics](./legacy-components.md) (MP) que oferece uma redação facial escalável na nuvem. A redação facial permite-lhe modificar o seu vídeo de modo a desfocar rostos de indivíduos selecionados. Pode querer utilizar o serviço de redação facial em cenários de segurança pública e media de notícias. Alguns minutos de imagens que contêm múltiplas faces podem demorar horas a redigir manualmente, mas com este serviço o processo de redação facial requer apenas alguns passos simples. Para mais informações, consulte [este](https://azure.microsoft.com/blog/azure-media-redactor/) blog.

Para mais detalhes sobre **o Azure Media Redator,** consulte o tópico de visão geral da [redação face.](media-services-face-redaction.md)

Este tópico mostra instruções passo a passo sobre como executar um fluxo de trabalho de redação completo usando Azure Media Services Explorer (AMSE) e Azure Media Redator Visualizer (ferramenta open source).

Para mais informações, consulte [este](https://azure.microsoft.com/blog/redaction-preview-available-globally) blog.

## <a name="azure-media-services-explorer-workflow"></a>Fluxo de trabalho Azure Media Services Explorer

A maneira mais fácil de começar com o Redator é usar a ferramenta AMSE de código aberto no GitHub. Pode executar um fluxo de trabalho simplificado através do modo **combinado** se não precisar de acesso às imagens de anotação ou jpg facial.

### <a name="download-and-setup"></a>Download e configuração

1. Descarregue a ferramenta AMSE v2 [daqui.](https://aka.ms/amseforv2)
1. Faça login na sua conta de Serviços de Mídia utilizando a sua chave de serviço.

    Para obter o nome de conta e as informações da chave, aceda ao [portal do Azure](https://portal.azure.com/) e selecione a sua conta AMS. Em seguida, selecione Definições > Teclas. A janela Gerir chaves mostra o nome da conta e as chaves primária e secundária são apresentadas. Copie os valores do nome da conta e a chave primária.

![O Screenshot mostra o Microsoft Azure Media Services onde pode introduzir o nome e a chave da sua conta.](./media/media-services-redactor-walkthrough/media-services-redactor-walkthrough001.png)

### <a name="first-pass--analyze-mode"></a>Primeiro passe – modo de análise

1. Faça upload do seu ficheiro de mídia através do Ativo –> Upload, ou através de arrasto e queda. 
1. Clique e processe o seu ficheiro de media utilizando o media Analytics (> Azure Media Redator - > Modo de Análise. 


![O Screenshot mostra um menu com o (s) Process Asset(s) com o Azure Media Redator.](./media/media-services-redactor-walkthrough/media-services-redactor-walkthrough002.png)

![Screenshot mostra Azure Media Redator com Primeiro Passe: Modo de análise selecionado.](./media/media-services-redactor-walkthrough/media-services-redactor-walkthrough003.png)

A saída incluirá um ficheiro json de anotações com dados de localização facial, bem como um jpg de cada face detetada. 

![A imagem mostra a saída da análise.](./media/media-services-redactor-walkthrough/media-services-redactor-walkthrough004.png)

### <a name="second-pass--redact-mode"></a>Segundo passe – modo de redação

1. Faça o upload do seu ativo de vídeo original para a saída a partir do primeiro passe e definido como um ativo primário. 

    ![A screenshot mostra o upload e o conjunto como botões primários.](./media/media-services-redactor-walkthrough/media-services-redactor-walkthrough005.png)

2. (Opcional) Faça o upload de um ficheiro 'Dance_idlist.txt' que inclui uma nova lista delimitada dos IDs que pretende redigir. 

    ![A screenshot mostra a opção de carregar o ficheiro de texto.](./media/media-services-redactor-walkthrough/media-services-redactor-walkthrough006.png)

3. (Opcional) Faça quaisquer edições para o annotations.jsem ficheiros como aumentar os limites da caixa de delimitação. 
4. Clique no ativo de saída a partir do primeiro passe, selecione o Redator e corra com o modo **Redact.** 

    ![A screenshot mostra Azure Media Redator com Second Pass: Modo Redact selecionado.](./media/media-services-redactor-walkthrough/media-services-redactor-walkthrough007.png)

5. Faça o download ou partilhe o ativo de saída redigido final. 

    ![A screenshot mostra o botão Descarregar.](./media/media-services-redactor-walkthrough/media-services-redactor-walkthrough008.png)

## <a name="azure-media-redactor-visualizer-open-source-tool"></a>Ferramenta de código open source do Azure Media Redator

Uma [ferramenta de visualizador](https://github.com/Microsoft/azure-media-redactor-visualizer) de código aberto foi concebida para ajudar os desenvolvedores a começar apenas com o formato de anotações com a análise e usando a saída.

Depois de clonar o repo, para executar o projeto, terá de descarregar o FFMPEG a partir do seu [site oficial.](https://ffmpeg.org/download.html)

Se for um desenvolvedor a tentar analisar os dados de anotação JSON, procure dentro de Models.MetaData para obter exemplos de código de amostra.

### <a name="set-up-the-tool"></a>Configurar a ferramenta

1.  Descarregue e construa toda a solução. 

    ![Screenshot mostra Build Solution selecionado a partir do menu.](./media/media-services-redactor-walkthrough/media-services-redactor-walkthrough009.png)

2.  Faça o download do FFMPEG a partir [daqui.](https://ffmpeg.org/download.html) Este projeto foi originalmente desenvolvido com a versão be1d324 (2016-10-04) com ligação estática. 
3.  Copie ffmpeg.exe e ffprobe.exe na mesma pasta de saída que AzureMediaRedactor.exe. 

    ![A screenshot mostra o conteúdo da pasta, incluindo ffmpeg e ffprobe.](./media/media-services-redactor-walkthrough/media-services-redactor-walkthrough010.png)

4. Corre AzureMediaRedactor.exe. 

### <a name="use-the-tool"></a>Use a ferramenta

1. Processe o seu vídeo na sua conta Azure Media Services com o Deputado Redator no modo Análise. 
2. Descarregue tanto o ficheiro de vídeo original como a saída do trabalho Redaction - Analisar. 
3. Executar a aplicação de visualizador e escolher os ficheiros acima. 

    ![A screenshot mostra ficheiros de upload do Azure Media Redator.](./media/media-services-redactor-walkthrough/media-services-redactor-walkthrough011.png)

4. Pré-visualizar o seu ficheiro. Selecione quais os rostos que gostaria de borrar através da barra lateral à direita. 
    
    ![O Screenshot mostra o Azure Media Redator onde pode visualizar e selecionar rostos para desfocar.](./media/media-services-redactor-walkthrough/media-services-redactor-walkthrough012.png)

5.  O campo de texto inferior atualizar-se-á com os IDs do rosto. Crie um ficheiro chamado "idlist.txt" com estes IDs como uma nova lista delimitada. 

    >[!NOTE]
    > O idlist.txt deve ser guardado na ANSI. Pode utilizar o bloco de notas para guardar na ANSI.
    
6.  Faça o upload deste ficheiro para o ativo de saída a partir do passo 1. Faça o upload do vídeo original para este ativo também e definido como ativo primário. 
7.  Executar o trabalho de Redaction neste ativo com o modo "Redact" para obter o vídeo final redigido. 

## <a name="next-steps"></a>Passos seguintes 

[!INCLUDE [media-services-learning-paths-include](../../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Enviar comentários
[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]

## <a name="related-links"></a>Ligações relacionadas
[Visão geral da Azure Media Services Analytics](./legacy-components.md)

[Azure Media Analytics demos](http://amslabs.azurewebsites.net/demos/Analytics.html)

[Anunciando a redação facial para a Azure Media Analytics](https://azure.microsoft.com/blog/azure-media-redactor/)