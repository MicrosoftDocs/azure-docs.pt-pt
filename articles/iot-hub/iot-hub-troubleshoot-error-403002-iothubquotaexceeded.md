---
title: Resolução de problemas Erro do Hub Azure IoT 403002 IoTHubQuotaExceed
description: Entenda como corrigir o erro 403002 IoTHubQuotaExceed
author: jlian
manager: briz
ms.service: iot-hub
services: iot-hub
ms.topic: troubleshooting
ms.date: 01/30/2020
ms.author: jlian
ms.openlocfilehash: 3521933baa8dc328910fbacd8b1b6768832fa5f1
ms.sourcegitcommit: 73fb48074c4c91c3511d5bcdffd6e40854fb46e5
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/31/2021
ms.locfileid: "106061320"
---
# <a name="403002-iothubquotaexceeded"></a>403002 IoTHubQuotaExceeded

Este artigo descreve as causas e soluções para **403002 erros IoTHubQuotaExceeded.**

## <a name="symptoms"></a>Sintomas

Todos os pedidos ao IoT Hub falham com o erro  **403002 IoTHubQuotaExceed**. No portal Azure, a lista de dispositivos do hub IoT não carrega.

## <a name="cause"></a>Causa

A quota de mensagem diária para o hub IoT é excedida. 

## <a name="solution"></a>Solução

[Atualize ou aumente o número de unidades no hub IoT](iot-hub-upgrade.md) ou aguarde o próximo dia utc para que a quota diária se atualização.

## <a name="next-steps"></a>Passos seguintes

* Para entender como as operações são contadas para a quota, tais como consultas gémeas e métodos diretos, consulte [os preços do Hub de Compreensão](iot-hub-devguide-pricing.md#charges-per-operation)
* Para configurar a monitorização para a utilização diária de quotas, crie um alerta com a métrica *Número Total de mensagens utilizadas*. Para obter instruções passo a passo, consulte [Configurar métricas e alertas com IoT Hub](tutorial-use-metrics-and-diags.md#set-up-metrics)