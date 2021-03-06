---
title: Compreenda os pontos finais personalizados do Azure IoT Hub | Microsoft Docs
description: Guia do desenvolvedor - usando consultas de encaminhamento para encaminhar mensagens dispositivo-a-nuvem para pontos finais personalizados.
author: wesmc7777
manager: philmea
ms.author: wesmc
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 04/09/2018
ms.openlocfilehash: 4ad57473e0950f031fbeadee2302f85557ed526f
ms.sourcegitcommit: aa00fecfa3ad1c26ab6f5502163a3246cfb99ec3
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/14/2021
ms.locfileid: "107388266"
---
# <a name="use-message-routes-and-custom-endpoints-for-device-to-cloud-messages"></a>Utilize rotas de mensagens e pontos finais personalizados para mensagens dispositivo-a-nuvem

[!INCLUDE [iot-hub-basic](../../includes/iot-hub-basic-partial.md)]

[O Encaminhamento](iot-hub-devguide-routing-query-syntax.md) de Mensagens IoT Hub permite que os utilizadores encaminhem mensagens dispositivo-a-nuvem para pontos finais virados para o serviço. O encaminhamento também fornece uma capacidade de consulta para filtrar os dados antes de encaminhá-lo para os pontos finais. Cada consulta de encaminhamento que configura tem as seguintes propriedades:

| Propriedade      | Descrição |
| ------------- | ----------- |
| **Nome**      | O nome único que identifica a consulta. |
| **Origem**    | A origem do fluxo de dados a ser agia. Por exemplo, telemetria do dispositivo. |
| **Condition** | A expressão de consulta para a consulta de encaminhamento que é executada contra as propriedades da aplicação de mensagens, propriedades do sistema, corpo de mensagem, etiquetas gémeas do dispositivo e propriedades gémeas do dispositivo para determinar se é uma correspondência para o ponto final. Para obter mais informações sobre a construção de uma consulta, consulte a [sintaxe de consulta de encaminhamento de mensagens](iot-hub-devguide-routing-query-syntax.md) |
| **Ponto final**  | O nome do ponto final onde o IoT Hub envia mensagens que correspondem à consulta. Recomendamos que escolha um ponto final na mesma região que o seu hub IoT. |

Uma única mensagem pode coincidir com a condição em várias consultas de encaminhamento, caso em que o IoT Hub entrega a mensagem ao ponto final associado a cada consulta correspondida. O IoT Hub também desafia automaticamente a entrega de mensagens, por isso, se uma mensagem corresponder a múltiplas consultas que tenham o mesmo destino, só é escrita uma vez para esse destino.

## <a name="endpoints-and-routing"></a>Pontos finais e encaminhamento

Um hub IoT tem um [ponto final incorporado](iot-hub-devguide-messages-read-builtin.md)padrão. Pode criar pontos finais personalizados para encaminhar mensagens para, ligando outros serviços nas subscrições que possui ao hub. O IoT Hub suporta atualmente contentores de armazenamento Azure, Centros de Eventos, filas de autocarros de serviço e tópicos de Service Bus como pontos finais personalizados.

Quando utiliza o encaminhamento e os pontos finais personalizados, as mensagens só são entregues no ponto final incorporado se não corresponderem a qualquer consulta. Para entregar mensagens no ponto final incorporado, bem como para um ponto final personalizado, adicione uma rota que envia mensagens para o ponto final de **eventos incorporados.**

> [!NOTE]
> * O IoT Hub só suporta escrever dados para os recipientes de armazenamento Azure como bolhas.
> * As filas e tópicos do Service Bus com **sessões** ou **deteção duplicada** não são suportados como pontos finais personalizados.
> * No portal Azure, pode criar pontos finais de encaminhamento personalizados apenas para recursos Azure que estejam na mesma subscrição que o seu hub. Pode criar pontos finais personalizados para recursos em outras subscrições que possui, mas os pontos finais personalizados devem ser configurados utilizando um método diferente do portal Azure.

Para obter mais informações sobre a criação de pontos finais personalizados no IoT Hub, consulte [os pontos finais do IoT Hub](iot-hub-devguide-endpoints.md).

Para obter mais informações sobre a leitura de pontos finais personalizados, consulte:

* Leitura dos recipientes de [armazenamento Azure](../storage/blobs/storage-blobs-introduction.md).

* Leitura de Centros de [Eventos.](../event-hubs/event-hubs-dotnet-standard-getstarted-send.md)

* Leitura das filas de [autocarros de serviço](../service-bus-messaging/service-bus-dotnet-get-started-with-queues.md).

* Ler [tópicos](../service-bus-messaging/service-bus-dotnet-how-to-use-topics-subscriptions.md)de Service Bus .

## <a name="next-steps"></a>Passos seguintes

* Para obter mais informações sobre os pontos finais do [IoT Hub, consulte os pontos finais do IoT Hub](iot-hub-devguide-endpoints.md).

* Para obter mais informações sobre o idioma de consulta que utiliza para definir consultas de encaminhamento, consulte [a sintaxe de consulta de encaminhamento de mensagens](iot-hub-devguide-routing-query-syntax.md).

* As [mensagens process IoT Hub dispositivo-nuvem usando](tutorial-routing.md) o tutorial de rotas mostram-lhe como usar consultas de encaminhamento e pontos finais personalizados.
