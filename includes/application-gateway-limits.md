---
author: vhorne
ms.service: application-gateway
ms.topic: include
ms.date: 03/04/2020
ms.author: victorh
ms.openlocfilehash: 1166585c7291c4fe0d78cbc9540e3f08f985db6c
ms.sourcegitcommit: d3bcd46f71f578ca2fd8ed94c3cdabe1c1e0302d
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/16/2021
ms.locfileid: "107589887"
---
| Recurso | Limite | Nota |
| --- | --- | --- |
| Gateway de Aplicação do Azure |1.000 por subscrição | |
| Configurações IP front-end |2 |1 pública e 1 privada |
| Portas frontais |100<sup>1</sup> | |
| Piscinas de endereços de back-end |100<sup>1</sup> | |
| Servidores de back-end por piscina |1200 | |
| Ouvintes HTTP |200<sup>1</sup> |Limitado a 100 ouvintes ativos que estão a encaminhar o tráfego. Ouvintes ativos = número total de ouvintes - ouvintes não ativos.<br>Se uma configuração padrão dentro de uma regra de encaminhamento for definida para encaminhar o tráfego (por exemplo, tem um ouvinte, uma piscina de backend e definições HTTP) então isso também conta como ouvinte.|
| Regras de equilíbrio de carga HTTP |100<sup>1</sup> | |
| Definições HTTP de back-end |100<sup>1</sup> | |
| Instâncias por gateway |V1 SKU - 32<br>V2 SKU - 125 | |
| Certificados SSL |100<sup>1</sup> |1 por ouvinte HTTP |
| Tamanho máximo do certificado SSL |V1 SKU - 10 KB<br>V2 SKU - 16 KB| |
| Certificados de autenticação |100 | |
| Certificados de raiz fidedignos |100 | |
| Pedido mínimo de tempo |1 segundo | |
| Pedido no máximo |24 horas | |
| Número de sites |100<sup>1</sup> |1 por ouvinte HTTP |
| Mapas de URL por ouvinte |1 | |
| Regras máximas baseadas em caminhos por mapa de URL|100||
| Redirecionar configurações |100<sup>1</sup>| |
| Número de conjuntos de regras de reescrita |400| |
| Número de configuração de cabeçalho ou URL por conjunto de regras de reescrita|40| |
| Número de condições por conjunto de regras de reescrita|40| |
| Conexões Simultâneas WebSocket |Gateways médios 20k<sup>2</sup><br> Grandes portais 50k<sup>2</sup>| |
| Comprimento máximo de URL|32KB| |
| Tamanho máximo do cabeçalho para HTTP/2 |16KB| |
| Tamanho máximo de upload de ficheiros, Standard |2 GB | |
| Tamanho máximo de upload de ficheiros WAF |Gateways V1 Medium WAF, 100 MB<br>V1 Grandes portais WAF, 500 MB<br>V2 WAF, 750 MB| |
| Limite de tamanho do corpo da WAF, sem ficheiros|128 KB||
| Regras personalizadas máximas da WAF|100||
| Exclusões máximas de WAF por Gateway de aplicação|40||

<sup>1</sup> No caso de SKUs ativados pela WAF, deve limitar o número de recursos a 40.

<sup>2</sup> Limite é por instância de Gateway de aplicação não por recurso de Gateway de aplicação.
