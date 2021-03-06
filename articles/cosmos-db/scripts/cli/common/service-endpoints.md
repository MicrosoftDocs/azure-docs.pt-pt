---
title: Criar uma conta Azure Cosmos com pontos finais de serviço de rede virtual
description: Criar uma conta Azure Cosmos com pontos finais de serviço de rede virtual
author: markjbrown
ms.author: mjbrown
ms.service: cosmos-db
ms.topic: sample
ms.date: 07/29/2020
ms.openlocfilehash: 6aa8da221818f807c29310f0b124b58ae70b853e
ms.sourcegitcommit: 4b0e424f5aa8a11daf0eec32456854542a2f5df0
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/20/2021
ms.locfileid: "107770776"
---
# <a name="create-an-azure-cosmos-account-with-virtual-network-service-endpoints-using-azure-cli"></a>Criar uma conta Azure Cosmos com pontos finais de serviço de rede virtual usando Azure CLI
[!INCLUDE[appliesto-all-apis](../../../includes/appliesto-all-apis.md)]

[!INCLUDE [azure-cli-prepare-your-environment.md](../../../../../includes/azure-cli-prepare-your-environment.md)]

- Este artigo requer a versão 2.9.1 ou posterior do Azure CLI. Se utilizar o Azure Cloud Shell, a versão mais recente já está instalada.

## <a name="sample-script"></a>Script de exemplo

Esta amostra cria uma nova rede virtual com uma sub-rede frontal e traseira e permite pontos finais de serviço para `Microsoft.AzureCosmosDB` . Em seguida, recupera o ID de recurso para esta sub-rede e aplica-o na conta Azure Cosmos e permite pontos finais de serviço para a conta.

> [!NOTE]
> Esta amostra demonstra a utilização de uma conta API Core (SQL). Para utilizar esta amostra para outras APIs, aplique os `enable-virtual-network` parâmetros e `virtual-network-rules` parâmetros no script abaixo no seu script específico da API.

[!code-azurecli-interactive[main](../../../../../cli_scripts/cosmosdb/common/service-endpoints.sh "Create an Azure Cosmos account with service endpoints.")]

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
| [az group delete](/cli/azure/resource#az_resource_delete) | Elimina um grupo de recursos, incluindo todos os recursos aninhados. |

## <a name="next-steps"></a>Passos seguintes

Para mais informações sobre o Azure Cosmos DB CLI, consulte [a documentação do Azure Cosmos DB CLI](/cli/azure/cosmosdb).

Todas as amostras de scriptS Azure Cosmos DB CLI podem ser encontradas no [Repositório Azure Cosmos DB CLI GitHub](https://github.com/Azure-Samples/azure-cli-samples/tree/master/cosmosdb).
