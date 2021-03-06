---
title: Tráfego de rotas para HA de aplicações - Azure CLI - Traffic Manager
description: Amostra de script Azure CLI - Tráfego de rota para alta disponibilidade de aplicações
services: traffic-manager
documentationcenter: traffic-manager
author: duongau
manager: twooley
tags: azure-infrastructure
ms.assetid: ''
ms.service: traffic-manager
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: traffic-manager
ms.date: 04/26/2018
ms.author: duau
ms.openlocfilehash: 151cfc4ffb984b25902ae6d5dd69516a5e974c93
ms.sourcegitcommit: 4b0e424f5aa8a11daf0eec32456854542a2f5df0
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/20/2021
ms.locfileid: "107788354"
---
# <a name="route-traffic-for-high-availability-of-applications-using-azure-cli"></a>Tráfego de rotas para elevada disponibilidade de aplicações utilizando O Azure CLI

Este script cria um grupo de recursos, dois planos de serviço de aplicações, duas aplicações web, um perfil de gestor de tráfego e dois pontos finais de gestor de tráfego. O Gestor de Tráfego direciona o tráfego para a aplicação numa região como a região primária, e para a região secundária quando a aplicação na região primária não está disponível. Antes de executar o script, tem de alterar os valores MyWebApp, MyWebAppL1 e MyWebAppL2 para valores únicos em todo o Azure. Depois de executar o script, pode aceder à aplicação na região primária com o URL mywebapp.trafficmanager.net.

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Script de exemplo

[!code-azurecli-interactive[main](../../../cli_scripts/traffic-manager/direct-traffic-for-increased-application-availability/direct-traffic-for-increased-application-availability.sh "Route traffic for high availability")]


## <a name="clean-up-deployment"></a>Limpar a implementação 

Após a execução da amostra de script, o comando seguinte pode ser usado para remover o grupo de recursos, app App Service e todos os recursos relacionados.

```azurecli
az group delete --name myResourceGroup1 --yes
az group delete --name myResourceGroup2 --yes
```

## <a name="script-explanation"></a>Explicação do script

Este script utiliza os seguintes comandos para criar um grupo de recursos, uma aplicação Web, um perfil do gestor de tráfego e todos os recursos relacionados. Cada comando na tabela liga à documentação específica do comando.

| Comando | Notas |
|---|---|
| [az group create](/cli/azure/group) | Cria um grupo de recursos no qual todos os recursos são armazenados. |
| [az appservice plan create](/cli/azure/appservice/plan) | Cria um plano do Serviço de Aplicações. Isto é como uma fazenda de servidores para a sua aplicação web Azure. |
| [webapp az criar](/cli/azure/webapp#az_webapp_create) | Cria uma aplicação web Azure dentro do plano de Serviço de Aplicações. |
| [az perfil de gestor de tráfego de rede criar](/cli/azure/network/traffic-manager/profile) | Cria um perfil do Gestor de Tráfego do Azure. |
| [az rede gestor de tráfego-manager criar](/cli/azure/network/traffic-manager/endpoint) | Adiciona um ponto final a um Perfil do Gestor de Tráfego do Azure. |

## <a name="next-steps"></a>Passos seguintes

Para obter mais informações sobre a CLI do Azure, veja [Documentação da CLI do Azure](/cli/azure).

As amostras de script cli do Serviço de Aplicações adicionais podem ser encontradas na documentação de [rede Azure Networking](../cli-samples.md).