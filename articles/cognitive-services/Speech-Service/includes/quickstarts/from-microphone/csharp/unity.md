---
title: 'Quickstart: Reconhecer a fala a partir de um microfone, C# (Unidade)- Serviço de fala'
titleSuffix: Azure Cognitive Services
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.custom: devx-track-csharp
ms.topic: include
ms.date: 04/02/2020
ms.author: erhopf
ms.openlocfilehash: 974f147492877f481616b43d67146a03814ada16
ms.sourcegitcommit: d95cab0514dd0956c13b9d64d98fdae2bc3569a0
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/25/2020
ms.locfileid: "91376691"
---
> [!NOTE]
> O Speech SDK for Unitity suporta o Windows Desktop (x86 e x64) ou a Plataforma Universal windows (x86, x64, ARM/ARM64), Android (x86, ARM32/64) e iOS (simulador x64, ARM32 e ARM64)

## <a name="prerequisites"></a>Pré-requisitos

Antes de começar:

> [!div class="checklist"]
> * [Criar um recurso de fala azul](../../../../overview.md#try-the-speech-service-for-free)
> * [Configurar o seu ambiente de desenvolvimento e criar um projeto vazio](../../../../quickstarts/setup-platform.md?tabs=unity&pivots=programming-language-csharp)
> * Certifique-se de que tem acesso a um microfone para captura de áudio

Se já fez isto, ótimo. Vamos continuar.

## <a name="create-a-unity-project"></a>Criar um projeto de unidade

1. Unidade Aberta. Se estiveres a usar a Unidade pela primeira vez, aparece a janela **do Centro de Unidade.** *<version number>* (Também pode abrir o Centro de Unidade diretamente para chegar a esta janela.)

   [![Janela do Centro de Unidade](~/articles/cognitive-services/Speech-Service/media/sdk/qs-csharp-unity-hub.png)](~/articles/cognitive-services/Speech-Service/media/sdk/qs-csharp-unity-hub.png#lightbox)
1. Selecione **New** (Novo). O **Create surge um novo projeto com** janela de *<version number>* unidade.

   [![Criar um novo projeto no Centro de Unidade](~/articles/cognitive-services/Speech-Service/media/sdk/qs-csharp-unity-create-a-new-project.png)](~/articles/cognitive-services/Speech-Service/media/sdk/qs-csharp-unity-create-a-new-project.png#lightbox)
1. Em **Nome do Projeto,** **insira a unidade do csharp**.
1. Em **Modelos**, se **o 3D** ainda não estiver selecionado, selecione-o.
1. No **Local,** selecione ou crie uma pasta para guardar o projeto.
1. Selecione **Criar**.

Depois de um pouco de tempo, a janela do Editor de Unidade aparece.



## <a name="add-ui"></a>Adicionar UI

Agora vamos adicionar uma UI mínima à nossa cena. Esta UI consiste num botão para acionar o reconhecimento da fala e um campo de texto para exibir o resultado. Na janela da [ **Hierarquia,** ](https://docs.unity3d.com/Manual/Hierarchy.html)mostra-se uma cena de amostra que a Unidade criou com o novo projeto.

1. No topo da janela **da hierarquia,** selecione **Create**  >  **UI**  >  **Button**.

   Esta ação cria três objetos de jogo que podes ver na janela **da Hierarquia:** um objeto **de botão,** um objeto **de tela** contendo o botão e um objeto **EventSystem.**

   [![Ambiente editor de unidade](~/articles/cognitive-services/Speech-Service/media/sdk/qs-csharp-unity-editor-window.png)](~/articles/cognitive-services/Speech-Service/media/sdk/qs-csharp-unity-editor-window.png#lightbox)

1. [Navegue na vista **cena** ](https://docs.unity3d.com/Manual/SceneViewNavigation.html) para que tenha uma boa vista da tela e do botão na vista [ **cena** ](https://docs.unity3d.com/Manual/UsingTheSceneView.html).

1. Na janela do [ **Inspetor** ](https://docs.unity3d.com/Manual/UsingTheInspector.html) (por defeito à direita), coloque as propriedades **Pos X** e **Pos Y** em **0,** de modo que o botão esteja centrado no meio da tela.

1. Na janela **hierarquia,** **selecione Criar**  >  **Texto UI**para criar um objeto de  >  **Text** **texto.**

1. Na janela do **Inspetor,** coloque as propriedades **Pos X** e **Pos Y** em **0** e **120,** e coloque as propriedades **largura** e **altura** em **240** e **120**. Estes valores garantem que o campo de texto e o botão não se sobrepõem.

Quando terminar, a vista **cena** deve ser semelhante a esta imagem:

[![Vista de cena no Editor de Unidade](~/articles/cognitive-services/Speech-Service/media/sdk/qs-csharp-unity-02-ui-inline.png)](~/articles/cognitive-services/Speech-Service/media/sdk/qs-csharp-unity-02-ui-inline.png#lightbox)

## <a name="add-the-sample-code"></a>Adicionar o código de exemplo

Para adicionar o código de script de amostra para o projeto Unidade, siga estes passos:

1. Na [janela Do Projeto,](https://docs.unity3d.com/Manual/ProjectView.html)selecione **Crie**  >  **o script C#** para adicionar um novo script C#.

   [![Janela do projeto no Editor de Unidade](~/articles/cognitive-services/Speech-Service/media/sdk/qs-csharp-unity-project-window.png)](~/articles/cognitive-services/Speech-Service/media/sdk/qs-csharp-unity-project-window.png#lightbox)
1. Diga o nome do `HelloWorld` guião.

1. Clique duas vezes `HelloWorld` para editar o script recém-criado.

   > [!NOTE]
   > Para configurar o editor de código a ser usado **Edit**pela Unidade para edição, selecione  >  **Editar Preferências**e, em seguida, vá para as preferências **de Ferramentas Externas.** Para mais informações, consulte o Manual do [Utilizador da Unidade.](https://docs.unity3d.com/Manual/Preferences.html)

1. Substitua o script existente pelo seguinte código:

   [!code-csharp[Quickstart Code](~/samples-cognitive-services-speech-sdk/quickstart/csharp/unity/from-microphone/Assets/Scripts/HelloWorld.cs#code)]

1. Encontre e substitua a cadeia `YourSubscriptionKey` pela tecla de subscrição do serviço Speech.

1. Encontre e substitua a cadeia `YourServiceRegion` pelo identificador da **Região** da [região](https://aka.ms/speech/sdkregion) associada à sua subscrição.

1. Guarde as alterações no script.

Agora volte ao Editor de Unidade e adicione o script como componente a um dos seus objetos de jogo:

1. Na janela **da hierarquia,** selecione o objeto **De lona.**

1. Na janela **do Inspetor,** selecione o botão **Adicionar Componente.**

   [![Janela do inspetor no Editor de Unidade](~/articles/cognitive-services/Speech-Service/media/sdk/qs-csharp-unity-inspector-window.png)](~/articles/cognitive-services/Speech-Service/media/sdk/qs-csharp-unity-inspector-window.png#lightbox)

1. Na lista de drop-down, procure o `HelloWorld` script que criamos acima e adicione-o. Uma secção **Hello World (Script)** aparece na janela do **Inspetor,** listando duas propriedades não ininitializadas, **texto de saída** e **botão Iniciar Reco**. Estas propriedades componentes de unidade combinam com as propriedades públicas da `HelloWorld` classe.

1. Selecione o apanhador de objetos da propriedade **Start Reco Button** (o ícone do pequeno círculo à direita da propriedade) e escolha o objeto **Botão** que criou anteriormente.

1. Selecione o selecionador de objetos da propriedade **De saída** e escolha o objeto **de texto** que criou anteriormente.

   > [!NOTE]
   > O botão também tem um objeto de texto aninhado. Certifique-se de que não o escolhe acidentalmente para a saída de texto (ou mude o nome de um dos objetos de texto utilizando o campo **Nome** na janela do **Inspetor** para evitar confusões).

## <a name="run-the-application-in-the-unity-editor"></a>Executar a aplicação no Editor de Unidade

Agora estás pronto para executar a candidatura dentro do Editor de Unidade.

1. Na barra de ferramentas Do Editor de Unidade (abaixo da barra de menu), selecione o botão **Reproduzir** (um triângulo de ponta direita).

1. Vá à [ **visualização do Jogo** ](https://docs.unity3d.com/Manual/GameView.html)e aguarde que o objeto De **texto** apresente **o botão Clique para reconhecer a fala**. (Apresenta **Novo Texto** quando a aplicação não começou ou não está pronta para responder.)

1. Selecione o botão e fale uma frase ou frase em inglês no microfone do computador. O seu discurso é transmitido ao serviço Desempatado e transcrito para texto, que aparece na visão do **Jogo.**

   [![Vista do jogo no Editor de Unidade](~/articles/cognitive-services/Speech-Service/media/sdk/qs-csharp-unity-03-output-inline.png)](~/articles/cognitive-services/Speech-Service/media/sdk/qs-csharp-unity-03-output-inline.png#lightbox)

1. Verifique a janela [ **consola** ](https://docs.unity3d.com/Manual/Console.html) para obter mensagens de depurar. Se a janela **'Consola'** não estiver a aparecer, vá à barra de menus e selecione **a Consola**Geral da Janela  >  **General**  >  **Console** para a exibir.

1. Quando terminar de reconhecer a fala, selecione o botão **Reproduzir** na barra de ferramentas Do Editor de Unidade para parar a aplicação.

## <a name="additional-options-to-run-this-application"></a>Opções adicionais para executar esta aplicação

Esta aplicação também pode ser implementada como uma aplicação Android, uma aplicação autónoma do Windows ou uma aplicação UWP.
Para mais informações, consulte o nosso [repositório de amostras.](https://aka.ms/csspeech/samples) A `quickstart/csharp-unity` pasta descreve a configuração para estes alvos adicionais.

## <a name="next-steps"></a>Passos seguintes

[!INCLUDE [Speech recognition basics](../../speech-to-text-next-steps.md)]

