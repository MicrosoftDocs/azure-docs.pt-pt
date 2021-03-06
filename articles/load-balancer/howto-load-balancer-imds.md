---
title: Recupere metadados do balançador de carga utilizando o Serviço de Metadados de Instância Azure (IMDS)
titleSuffix: Azure Load Balancer
description: Começa a aprender a recuperar metadados do balancer de carga utilizando o Serviço de Metadados de Caso Azure.
services: load-balancer
author: asudbring
ms.service: load-balancer
ms.topic: how-to
ms.date: 02/12/2021
ms.author: allensu
ms.openlocfilehash: 95d0e1ceb9e05ce58f388c3f88dc98b2cf6a0cc5
ms.sourcegitcommit: 32e0fedb80b5a5ed0d2336cea18c3ec3b5015ca1
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/30/2021
ms.locfileid: "105559592"
---
# <a name="retrieve-load-balancer-metadata-using-the-azure-instance-metadata-service-imds"></a>Recupere metadados do balançador de carga utilizando o Serviço de Metadados de Instância Azure (IMDS)

## <a name="prerequisites"></a>Pré-requisitos

* Utilize a [versão API mais recente](../virtual-machines/windows/instance-metadata-service.md?tabs=windows#supported-api-versions) para o seu pedido.

## <a name="sample-request-and-response"></a>Pedido e resposta da amostra
> [!IMPORTANT]
> Este exemplo contorna os proxies. **Tem de** contornar os proxies ao consultar o IMDS. Para mais informações, consulte [Proxies.](../virtual-machines/windows/instance-metadata-service.md?tabs=windows#proxies)
### <a name="windows"></a>[Windows](#tab/windows/)

```powershell
Invoke-RestMethod -Headers @{"Metadata"="true"} -Method GET -NoProxy -Uri "http://169.254.169.254:80/metadata/loadbalancer?api-version=2020-10-01" | ConvertTo-Json
```
> [!NOTE]
> O parâmetro -NoProxy foi introduzido no PowerShell 6.0. Se estiver a utilizar uma versão mais antiga do PowerShell, remova -NoProxy no organismo de pedido e certifique-se de que não está a utilizar um representante enquanto recupera informações do IMDS. Saiba mais [aqui.](../virtual-machines/windows/instance-metadata-service.md?tabs=windows#proxies)
> 
### <a name="linux"></a>[Linux](#tab/linux/)

```bash
curl -H "Metadata:true" --noproxy "*" "http://169.254.169.254:80/metadata/loadbalancer?api-version=2020-10-01"
```

---
### <a name="sample-response"></a>Resposta de amostra

```json
{
   "loadbalancer": {
    "publicIpAddresses":[
      {
         "frontendIpAddress":"51.0.0.1",
         "privateIpAddress":"10.1.0.4"
      }
   ],
   "inboundRules":[
      {
         "frontendIpAddress":"50.0.0.1",
         "protocol":"tcp",
         "frontendPort":80,
         "backendPort":443,
         "privateIpAddress":"10.1.0.4"
      },
      {
         "frontendIpAddress":"2603:10e1:100:2::1:1",
         "protocol":"tcp",
         "frontendPort":80,
         "backendPort":443,
         "privateIpAddress":"ace:cab:deca:deed::1"
      }
   ],
   "outboundRules":[
      {
         "frontendIpAddress":"50.0.0.1",
         "privateIpAddress":"10.1.0.4"
      },
      {
         "frotendIpAddress":"2603:10e1:100:2::1:1",
         "privateIpAddress":"ace:cab:deca:deed::1"
      }
    ]
   }
}

```

## <a name="next-steps"></a>Passos seguintes
[Códigos de erro comuns e etapas de resolução de problemas](troubleshoot-load-balancer-imds.md)

Saiba mais sobre [o Serviço de Metadados Azure Instance](../virtual-machines/windows/instance-metadata-service.md)

[Recupere todos os metadados para um exemplo](../virtual-machines/windows/instance-metadata-service.md?tabs=windows#access-azure-instance-metadata-service)

[Implementar um equilibrador de carga padrão](quickstart-load-balancer-standard-public-portal.md)