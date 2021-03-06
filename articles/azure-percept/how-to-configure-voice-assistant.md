---
title: Configure a aplicação de assistente de voz usando O Hub IoT do Azure
description: Configure a aplicação de assistente de voz usando O Hub IoT do Azure
author: elqu20
ms.author: v-elqu
ms.service: azure-percept
ms.topic: how-to
ms.date: 02/15/2021
ms.custom: template-how-to
ms.openlocfilehash: b22ef4ee0a8b5978bb2ec1c02fadf368815f3014
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/20/2021
ms.locfileid: "102095787"
---
# <a name="configure-voice-assistant-application-using-azure-iot-hub"></a>Configure a aplicação de assistente de voz usando O Hub IoT do Azure

Este artigo descreve como configurar a sua aplicação de assistente de voz usando o IoT Hub. Para um tutorial passo a passo que o guia através do processo de criação de um assistente de voz usando o modelo de demonstração, consulte [Build a no-code voice assistant with Azure Percept Studio e Azure Percept Audio](./tutorial-no-code-speech.md).

## <a name="update-your-voice-assistant-configuration"></a>Atualize a configuração do seu assistente de voz

1. Abra o [portal Azure](https://portal.azure.com) e **escreva O Hub IoT** na barra de pesquisa. Clique no ícone para abrir a página IoT Hub.

1. Na página IoT Hub, selecione o Hub IoT para o qual o seu dispositivo foi a provisionado.

1. Selecione **IoT Edge** sob **Gestão Automática de Dispositivos** no menu de navegação à esquerda para visualizar todos os dispositivos ligados ao seu Hub IoT.

1. Selecione o dispositivo para o qual a sua aplicação de assistente de voz foi implantada.

1. Clique em **Módulos De Conjunto**.

    :::image type="content" source="./media/manage-voice-assistant-using-iot-hub/set-modules.png" alt-text="Screenshot da página do dispositivo com módulos de conjunto em destaque.":::

1. Verifique se a seguinte entrada está presente na secção **Credenciais de Registo de Contentores.** Adicione credenciais se necessário.

    |Name|Endereço|Nome de utilizador|Palavra-passe|
    |----|-------|--------|--------|
    |azureedgedevices|azureedgedevices.azurecr.io|devkitprivatepreviewpull|

1. Na secção **módulos IoT Edge,** selecione **azureearspeechclientmodule**.

    :::image type="content" source="./media/manage-voice-assistant-using-iot-hub/modules.png" alt-text="Screenshot mostrando a lista de todos os módulos IoT Edge no dispositivo.":::

1. Clique no **separador Definições** do Módulo. Verifique a seguinte configuração:

    URI de imagem|Reiniciar a política|Estado desejado
    ---------|--------------|--------------
    mcr.microsoft.com/azureedgedevices/azureearspeechclientmodule:preload-devkit|sempre|correndo

    Se as suas definições não corresponderem, edite-as e clique em **Atualizar**.

1. Clique no separador **Variáveis ambiente.** Verifique se não existem variáveis ambientais definidas.

1. Clique no **separador Definições Gémeas** do Módulo. Atualizar a secção **deconfigs** de discurso da seguinte forma:

    ```
    "speechConfigs": {
        "appId": "<Application id for custom command project>",
        "key": "<Speech Resource key for custom command project>",
        "region": "<Region for the speech service>",
        "keywordModelUrl": "https://aedsamples.blob.core.windows.net/speech/keyword-tables/computer.table",
        "keyword": "computer"
    }
    ```

    > [!NOTE]
    > A palavra-chave usada acima é uma palavra-chave disponível publicamente por defeito. Se desejar utilizar a sua própria palavra-chave, pode adicionar a sua própria palavra-chave personalizada, enviando um ficheiro de mesa criado para o armazenamento de bolhas. O armazenamento de blobs precisa de ser configurado com acesso anónimo a contentores ou acesso anónimo à bolha.

## <a name="how-to-find-out-appid-key-and-region"></a>Como descobrir appId, chave e região

Para localizar o seu **appID,** **chave** e **região,** vá ao [Speech Studio:](https://speech.microsoft.com/)

1. Inscreva-se e selecione o recurso de fala apropriado.
1. Na página inicial do **Estúdio de Discurso,** clique em **Comandos Personalizados** em **Assistentes de Voz**.
1. Selecione o seu projeto-alvo.

    :::image type="content" source="./media/manage-voice-assistant-using-iot-hub/project.png" alt-text="Screenshot da página do projeto no Estúdio da Fala.":::

1. Clique em **Definições** no painel do menu à esquerda.
1. O **appID** e **a chave** estarão localizados no **separador Definições Gerais.**

    :::image type="content" source="./media/manage-voice-assistant-using-iot-hub/general-settings.png" alt-text="Screenshot das definições gerais do projeto de fala.":::

1. Para encontrar a sua **região,** abra o separador **recursos LUIS** dentro das definições. A **seleção de recursos de autoria** conterá informações sobre a região.

    :::image type="content" source="./media/manage-voice-assistant-using-iot-hub/luis-resources.png" alt-text="Screenshot dos recursos do projeto de fala LUIS.":::

1. Depois de inserir o seu **discursoConfigs** informações, clique em **Update**.

1. Clique no separador **Rotas** na parte superior da página de **módulos set.** Certifique-se de que tem uma rota com o seguinte valor:

    ```
    FROM /messages/modules/azureearspeechclientmodule/outputs/* INTO $upstream
    ```

    Adicione a rota se não existir.

1. Clique em **Rever + Criar**.

1. Clique em **Criar**.


## <a name="next-steps"></a>Passos seguintes

Depois de atualizar a configuração do seu assistente de voz, volte à demonstração no Azure Percept Studio para interagir com a aplicação.

