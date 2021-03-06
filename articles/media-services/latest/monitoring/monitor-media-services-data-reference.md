---
title: Referência de dados dos Serviços de Mídia de Monitorização
description: Material de referência importante necessário quando monitoriza os Serviços de Mídia
author: IngridAtMicrosoft
ms.author: inhenkel
manager: femila
ms.topic: reference
ms.service: media-services
ms.custom: subject-monitoring
ms.date: 04/21/2021
ms.openlocfilehash: 3fd7b8013ec67d718f308ccd1b72a6f90012e02e
ms.sourcegitcommit: 2aeb2c41fd22a02552ff871479124b567fa4463c
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/22/2021
ms.locfileid: "107873060"
---
# <a name="monitoring-media-services-data-reference"></a>Referência de dados dos Serviços de Mídia de Monitorização

Este artigo abrange os dados que são úteis para a monitorização dos Serviços de Comunicação Social. Para obter mais informações sobre todas as métricas da plataforma suportadas no Azure Monitor, [reveja as métricas suportadas com o Azure Monitor](../../../azure-monitor/essentials/metrics-supported.md).

## <a name="metrics"></a>Métricas

As métricas são recolhidas a intervalos regulares, quer o valor mude ou não. São úteis para alertar porque podem ser amostrados com frequência, e um alerta pode ser disparado rapidamente com uma lógica relativamente simples.


Os Serviços de Comunicação Social apoiam métricas de monitorização dos seguintes recursos:

|Tipo métrico | Fornecedor de recursos / Espaço de nomes tipo<br/> e ligação com métricas individuais |
|-------|-----|
| Serviços de Mídia Geral | [Geral](/azure/azure-monitor/essentials/metrics-supported#microsoftmediamediaservices) |
| Eventos em Direto | [Microsoft.Media/mediaservices/liveEvents](/azure/azure-monitor/essentials/metrics-supported#microsoftmediamediaservicesliveevents) 
| Pontos Finais de Transmissão em fluxo | [Microsoft.Media/mediaservices/streamingEndpoints,](/azure/azure-monitor/essentials/metrics-supported#microsoftmediamediaservicesstreamingendpoints)que são relevantes para o [Streaming Endpoints REST API](/rest/api/media/streamingendpoints). 


Deve também rever [as quotas e limites de conta.](../limits-quotas-constraints-reference.md)


## <a name="metric-dimensions"></a>Dimensões métricas

Para obter mais informações sobre as dimensões métricas, consulte [métricas multidimensionais.](../../../azure-monitor/essentials/data-platform-metrics.md#multi-dimensional-metrics)

Os serviços de comunicação têm as seguintes dimensões métricas.  São autoexplicativos com base nas métricas que suportam.  Consulte as [ligações métricas](#metrics) acima para obter mais informações.   
- OutputFormat
- HttpStatusCode 
- CódigoDoErro 
- Nome de track 

## <a name="resource-logs"></a>Registos do recurso

Os registos de recursos fornecem dados ricos e frequentes sobre o funcionamento de um recurso Azure. Para mais informações, consulte [Como recolher e consumir dados de registo dos seus recursos Azure.](../../../azure-monitor/essentials/platform-logs-overview.md)

Os Serviços de Comunicação Social suportam os seguintes registos de recursos: [Microsoft.Media/mediaservices](/azure/azure-monitor/essentials/resource-logs-categories#microsoftmediamediaservices)

## <a name="schemas"></a>Esquemas

Para uma descrição detalhada do esquema de registos de diagnóstico de nível superior, consulte [serviços, esquemas e categorias de Registos de Diagnóstico Azure](../../../azure-monitor/essentials/resource-logs-schema.md).

## <a name="key-delivery-log-schema-properties"></a>Principais propriedades do esquema de registo de entrega

Estas propriedades são específicas do esquema de registo de entrega de chaves.

|Nome|Descrição|
|---|---|
|keyId|A identificação da chave solicitada.|
|teclaType|Pode ser um dos seguintes valores: "Claro" (sem encriptação), "FairPlay", "PlayReady" ou "Widevine".|
|nome de política|O nome do Gestor de Recursos Azure da apólice.|
|tokenType|O tipo simbólico.|
|statusMessage|A mensagem de estado.|

### <a name="example"></a>Exemplo

Propriedades do esquema de pedidos de entrega chave.

```json
{
    "time": "2019-01-11T17:59:10.4908614Z",
    "resourceId": "/SUBSCRIPTIONS/00000000-0000-0000-0000-0000000000/RESOURCEGROUPS/SBKEY/PROVIDERS/MICROSOFT.MEDIA/MEDIASERVICES/SBDNSTEST",
    "operationName": "MICROSOFT.MEDIA/MEDIASERVICES/CONTENTKEYS/READ",
    "operationVersion": "1.0",
    "category": "KeyDeliveryRequests",
    "resultType": "Succeeded",
    "resultSignature": "OK",
    "durationMs": 315,
    "identity": {
        "authorization": {
            "issuer": "http://testacs",
            "audience": "urn:test"
        },
        "claims": {
            "urn:microsoft:azure:mediaservices:contentkeyidentifier": "3321e646-78d0-4896-84ec-c7b98eddfca5",
            "iss": "http://testacs",
            "aud": "urn:test",
            "exp": "1547233138"
        }
    },
    "level": "Informational",
    "location": "uswestcentral",
    "properties": {
        "requestId": "b0243468-d8e5-4edf-a48b-d408e1661050",
        "keyType": "Clear",
        "keyId": "3321e646-78d0-4896-84ec-c7b98eddfca5",
        "policyName": "56a70229-82d0-4174-82bc-e9d3b14e5dbf",
        "tokenType": "JWT",
        "statusMessage": "OK"
    }
} 
```

```json
 {
    "time": "2019-01-11T17:59:33.4676382Z",
    "resourceId": "/SUBSCRIPTIONS/00000000-0000-0000-0000-0000000000/RESOURCEGROUPS/SBKEY/PROVIDERS/MICROSOFT.MEDIA/MEDIASERVICES/SBDNSTEST",
    "operationName": "MICROSOFT.MEDIA/MEDIASERVICES/CONTENTKEYS/READ",
    "operationVersion": "1.0",
    "category": "KeyDeliveryRequests",
    "resultType": "Failed",
    "resultSignature": "Unauthorized",
    "durationMs": 2,
    "level": "Error",
    "location": "uswestcentral",
    "properties": {
        "requestId": "875af030-b77c-416b-b7e1-58f23ebec182",
        "keyType": "Clear",
        "keyId": "3321e646-78d0-4896-84ec-c7b98eddfca5",
        "policyName": "56a70229-82d0-4174-82bc-e9d3b14e5dbf",
        "tokenType": "None",
        "statusMessage": "No token present in authorization header or URL."
    }
} 
```

>[!NOTE]
> Widevine é um serviço fornecido pela Google Inc. e sujeito aos termos de serviço e Política de Privacidade da Google, Inc.

## <a name="next-steps"></a>Passos seguintes

[!INCLUDE [monitoring-next-steps](../includes/monitoring-next-steps.md)]
