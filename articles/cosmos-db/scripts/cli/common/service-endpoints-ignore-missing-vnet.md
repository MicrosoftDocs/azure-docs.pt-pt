---
title: Ligue uma conta Azure Cosmos existente com pontos finais de serviço de rede virtual
description: Ligue uma conta Azure Cosmos existente com pontos finais de serviço de rede virtual
author: markjbrown
ms.author: mjbrown
ms.service: cosmos-db
ms.topic: sample
ms.date: 07/29/2020
ms.openlocfilehash: eae343b0ac85f3fdf6d0f6d52c7afbb91f401df4
ms.sourcegitcommit: 4b0e424f5aa8a11daf0eec32456854542a2f5df0
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/20/2021
ms.locfileid: "107770789"
---
# <a name="connect-an-existing-azure-cosmos-account-with-virtual-network-service-endpoints-using-azure-cli"></a>Ligue uma conta Azure Cosmos existente com pontos finais de serviço de rede virtual usando Azure CLI
[!INCLUDE[appliesto-all-apis](../../../includes/appliesto-all-apis.md)]

[!INCLUDE [azure-cli-prepare-your-environment.md](../../../../../includes/azure-cli-prepare-your-environment.md)]

- Este artigo requer a versão 2.9.1 ou posterior do Azure CLI. Se utilizar o Azure Cloud Shell, a versão mais recente já está instalada.

## <a name="sample-script"></a>Script de exemplo

Esta amostra destina-se a mostrar como ligar uma conta Azure Cosmos existente a uma nova rede virtual existente onde a sub-rede ainda não está configurada para pontos finais de serviço utilizando o `ignore-missing-vnet-service-endpoint` parâmetro. Isto permite que a configuração da conta Cosmos seja concluída sem erro antes da configuração da sub-rede da rede virtual estar concluída. Uma vez concluída a configuração da sub-rede, a conta Cosmos será então acessível através da sub-rede configurada.

> [!NOTE]
> Esta amostra demonstra a utilização de uma conta API SQL (Core). Para utilizar esta amostra para outras APIs, aplique os `enable-virtual-network` parâmetros e `virtual-network-rules` parâmetros no script abaixo no seu script específico da API.

[!code-azurecli-interactive[main](../../../../../cli_scripts/cosmosdb/common/service-endpoints-ignore-missing-vnet.sh "Create an Azure Cosmos account with service endpoints.")]

## <a name="clean-up-deployment"></a>Limpar a implementação

Depois de executar o script de exemplo, pode ser utilizado o seguinte comando para remover o grupo de recursos e todos os recursos associados ao mesmo.

```azurecli-interactive
az group delete --name $resourceGroupName
```

## <a name="script-explanation"></a>Explicação do script

Este script utiliza os seguintes comandos. Cada comando na tabela liga à documentação específica do comando.

| Comando | Notas |
|---|---|
| [az group create](/cli/azure/group#az_group_create) | Cria um grupo de recursos no qual todos os recursos são armazenados. |
| [az network vnet create](/cli/azure/network/vnet#az_network_vnet_create) | Cria uma rede virtual Azure. |
| [az network vnet subnet create](/cli/azure/network/vnet/subnet#az_network_vnet_subnet_create) | Cria uma sub-rede para uma rede virtual Azure. |
| [az rede vnet subnet show](/cli/azure/network/vnet/subnet#az_network_vnet_subnet_show) | Devolve uma sub-rede para uma rede virtual Azure. |
| [az cosmosdb create](/cli/azure/cosmosdb#az_cosmosdb_create) | Cria uma conta do Azure Cosmos DB. |
| [az network vnet subnet update](/cli/azure/network/vnet/subnet#az_network_vnet_subnet_update) | Atualiza uma sub-rede para uma rede virtual Azure. |
| [az group delete](/cli/azure/resource#az_resource_delete) | Elimina um grupo de recursos, incluindo todos os recursos aninhados. |

## <a name="next-steps"></a>Passos seguintes

Para mais informações sobre o Azure Cosmos DB CLI, consulte [a documentação do Azure Cosmos DB CLI](/cli/azure/cosmosdb).

Todas as amostras de scriptS Azure Cosmos DB CLI podem ser encontradas no [Repositório Azure Cosmos DB CLI GitHub](https://github.com/Azure-Samples/azure-cli-samples/tree/master/cosmosdb).
