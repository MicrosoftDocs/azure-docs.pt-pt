---
title: Use o VS Cloud Explorer para gerir mensagens de dispositivoS Azure IoT Hub
description: Aprenda a usar o Cloud Explorer para o Visual Studio para monitorizar o dispositivo para mensagens na nuvem e enviar mensagens de nuvem para o dispositivo no Azure IoT Hub.
author: shizn
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 08/20/2019
ms.author: xshi
ms.openlocfilehash: 8461a77d06a63c2ac319323a91b5577ca4dce1cf
ms.sourcegitcommit: 590f14d35e831a2dbb803fc12ebbd3ed2046abff
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/16/2021
ms.locfileid: "107567035"
---
# <a name="use-cloud-explorer-for-visual-studio-to-send-and-receive-messages-between-your-device-and-iot-hub"></a>Utilize o Cloud Explorer para o Estúdio Visual para enviar e receber mensagens entre o seu dispositivo e o IoT Hub

![Diagrama de ponta a ponta](./media/iot-hub-visual-studio-cloud-device-messaging/e-to-e-diagram.png)

Neste artigo, aprende-se a usar o Cloud Explorer para o Visual Studio para monitorizar mensagens de dispositivo para nuvem e para enviar mensagens cloud-to-device. As mensagens dispositivo-nuvem podem ser dados de sensores que o seu dispositivo recolhe e depois enviam para o seu Hub IoT. As mensagens cloud-to-device podem ser comandos que o seu Hub IoT envia para o seu dispositivo. Por exemplo, piscar um LED que esteja ligado ao seu dispositivo.

[O Cloud Explorer](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.CloudExplorerForVS) é uma extensão útil do Visual Studio que lhe permite ver os seus recursos Azure, inspecionar as suas propriedades e realizar ações de desenvolvimento chave a partir do Estúdio Visual. Este artigo centra-se em como usar o Cloud Explorer para enviar e receber mensagens entre o seu dispositivo e o seu hub.

[!INCLUDE [iot-hub-basic](../../includes/iot-hub-basic-partial.md)]

## <a name="prerequisites"></a>Pré-requisitos

- Uma subscrição ativa do Azure.

- Um Hub Azure IoT sob a sua assinatura.

- Microsoft Visual Studio 2017 Update 9 ou mais tarde. Este artigo utiliza [o Visual Studio 2019](https://www.visualstudio.com/vs/).

- O componente Cloud Explorer do Instalador visual do Estúdio, que é selecionado por padrão com Azure Workload.

## <a name="update-cloud-explorer-to-latest-version"></a>Atualizar o Cloud Explorer para a versão mais recente

O componente Cloud Explorer do Visual Studio Installer para o Visual Studio 2017 suporta apenas mensagens de monitorização de dispositivos para nuvem e nuvem-para-dispositivo. Para utilizar o Visual Studio 2017, descarregue e instale o mais recente [Cloud Explorer](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.CloudExplorerForVS).

## <a name="sign-in-to-access-your-hub"></a>Inscreva-se para aceder ao seu hub

Para aceder ao seu hub, siga estes passos:

1. No Estúdio Visual, selecione **View**  >  **Cloud Explorer** para abrir o Cloud Explorer.

1. Selecione o ícone de Gestão de Conta para mostrar as suas subscrições.

    ![Ícone de gestão de conta](media/iot-hub-visual-studio-cloud-device-messaging/account-management-icon.png)

1. Se você está inscrito no Azure, as suas contas aparecem. Para entrar no Azure pela primeira vez, escolha **Adicionar uma conta**.

1. Selecione as subscrições Azure que pretende utilizar e escolha **Aplicar**.

1. Expanda a sua subscrição e, em seguida, **expanda os Hubs IoT**.  Debaixo de cada centro, podes ver os teus dispositivos para aquele centro.

    ![Lista de dispositivos](media/iot-hub-visual-studio-cloud-device-messaging/hub-device-list.png)

## <a name="monitor-device-to-cloud-messages"></a>Monitorizar mensagens dispositivo-nuvem

Para monitorizar as mensagens enviadas do seu dispositivo para o seu IoT Hub, siga estes passos:

1. Clique com o botão direito do seu Hub ou dispositivo IoT e selecione **Iniciar a Monitorização da Mensagem D2C**.

    ![Iniciar a monitorização da mensagem D2C](media/iot-hub-visual-studio-cloud-device-messaging/start-monitoring-d2c-message-vs2019.png)

1. As mensagens monitorizadas aparecem na **Saída**.

    ![Resultado da mensagem D2C](media/iot-hub-visual-studio-cloud-device-messaging/monitor-d2c-message-result-vs2019.png)

1. Para parar a monitorização, clique com o botão direito em qualquer Hub ou dispositivo IoT e selecione **Stop Monitoring D2C Message**.

## <a name="send-cloud-to-device-messages"></a>Enviar mensagens da cloud para o dispositivo

Para enviar uma mensagem do seu Hub IoT para o seu dispositivo, siga estes passos:

1. Clique com o botão direito no seu dispositivo e selecione **Enviar mensagem C2D**.

1. Introduza a mensagem na caixa de entrada.

    ![Enviar mensagem C2D](media/iot-hub-visual-studio-cloud-device-messaging/send-c2d-message-test.png)

    Os resultados aparecem na **Saída**.

    ![Enviar resultado da mensagem C2D](media/iot-hub-visual-studio-cloud-device-messaging/send-c2d-message-result-vs2019.png)

## <a name="next-steps"></a>Passos seguintes

Aprendeu a monitorizar mensagens de dispositivo para nuvem e a enviar mensagens nuvem-a-dispositivo entre o seu dispositivo IoT e o Azure IoT Hub.

[!INCLUDE [iot-hub-get-started-next-steps](../../includes/iot-hub-get-started-next-steps.md)]