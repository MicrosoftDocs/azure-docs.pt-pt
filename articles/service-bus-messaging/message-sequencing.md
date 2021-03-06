---
title: Sequenciação de mensagem de autocarro da Azure Service e os horários | Microsoft Docs
description: Este artigo explica como preservar a sequenciação e encomenda (com os timetamps) de mensagens Azure Service Bus.
ms.topic: article
ms.date: 04/14/2021
ms.openlocfilehash: 3d5300568232afae1238445113d60eda8cdb2f1b
ms.sourcegitcommit: 3b5cb7fb84a427aee5b15fb96b89ec213a6536c2
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/14/2021
ms.locfileid: "107497103"
---
# <a name="message-sequencing-and-timestamps"></a>Sequência de mensagens e carimbos de data/hora

Sequenciação e timestamping são duas funcionalidades que são sempre ativadas em todas as entidades do Service Bus e superam através `SequenceNumber` das propriedades e propriedades de `EnqueuedTimeUtc` mensagens recebidas ou navegadas.

Para os casos em que a ordem absoluta das mensagens é significativa e/ou em que um consumidor necessita de um identificador único de confiança para mensagens, o corretor carimba mensagens com um número de sequência sem lacunas, aumentando o número de sequências em relação à fila ou tópico. Para entidades divididas, o número da sequência é emitido em relação à partição.

O valor **SequenceNumber** é um número inteiro único de 64 bits atribuído a uma mensagem, uma vez que é aceite e armazenado pelo corretor e funções como seu identificador interno. Para entidades divididas, os 16 bits mais altos refletem o identificador de partição. Os números de sequência passam para zero quando o intervalo de 48/64 bits está esgotado.

O número de sequência pode ser confiado como um identificador único, uma vez que é atribuído por uma autoridade central e neutra e não por clientes. Também representa a verdadeira ordem de chegada, e é mais precisa do que um carimbo de tempo como critério de encomenda, porque os selos temporais podem não ter uma resolução suficientemente alta a taxas de mensagem extremas e podem estar sujeitos a (ainda que mínimo) distorcer o relógio em situações em que a propriedade do corretor transite entre nós.

A ordem de chegada absoluta importa, por exemplo, em cenários comerciais em que um número limitado de bens oferecidos são servidos numa base de primeiro a chegar, enquanto os fornecimentos duram; venda de bilhetes de concerto são um exemplo.

A capacidade de estampagem de tempo funciona como uma autoridade neutra e de confiança que captura com precisão a hora utc de chegada de uma mensagem, refletida na propriedade **EnqueuedTimeUtc.** O valor é útil se um cenário de negócio depender de prazos, tais como se um item de trabalho foi submetido numa determinada data antes da meia-noite, mas o processamento está muito atrás do atraso da fila.

## <a name="scheduled-messages"></a>Mensagens agendadas

Pode submeter mensagens para uma fila ou tópico para processamento adiado; por exemplo, para agendar uma tarefa para ficar disponível para processamento por um sistema a determinada hora. Esta capacidade realiza um programador de tempo distribuído de confiança.

As mensagens programadas não se materializam na fila até à hora definida. Antes desse tempo, as mensagens agendadas podem ser canceladas. O cancelamento elimina a mensagem.

Pode agendar mensagens usando qualquer um dos nossos clientes de duas formas:
- Use a API de envio regular, mas coloque a `ScheduledEnqueueTimeUtc` propriedade na mensagem antes de enviar.
- Utilize a mensagem de horário API, passe a mensagem normal e a hora programada. Isto irá devolver o **SequenceNumber** da mensagem agendada, que poderá utilizar mais tarde para cancelar a mensagem agendada, se necessário. 

As mensagens programadas e os seus números de sequência também podem ser descobertos através da [navegação por mensagens](message-browsing.md).

O **SequenceNumber** para uma mensagem agendada só é válido enquanto a mensagem estiver neste estado. À medida que a mensagem transita para o estado ativo, a mensagem é anexada à fila como se tivesse sido encostado no instante atual, o que inclui a atribuição de um novo **SequenceNumber**.

Como a funcionalidade está ancorada em mensagens individuais e as mensagens só podem ser encostadas uma vez, a Service Bus não suporta horários recorrentes de mensagens.

## <a name="next-steps"></a>Passos seguintes

Para saber mais sobre as mensagens do Service Bus, consulte os seguintes tópicos:

* [Filas, tópicos e subscrições do Service Bus](service-bus-queues-topics-subscriptions.md)
* [Introdução às filas do Service Bus](service-bus-dotnet-get-started-with-queues.md)
* [Como utilizar os tópicos e as subscrições do Service Bus](service-bus-dotnet-how-to-use-topics-subscriptions.md)
