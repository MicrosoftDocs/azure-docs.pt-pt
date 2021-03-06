---
title: Trabalhe com chaves de conta e cordas de conexão para uma conta Azure Cosmos
description: Trabalhe com chaves de conta e cordas de conexão para uma conta Azure Cosmos
author: markjbrown
ms.author: mjbrown
ms.service: cosmos-db
ms.topic: sample
ms.date: 07/29/2020
ms.openlocfilehash: bef0501704a3c9c400d998b6e84cf1cabb839dd1
ms.sourcegitcommit: 4b0e424f5aa8a11daf0eec32456854542a2f5df0
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/20/2021
ms.locfileid: "107770908"
---
# <a name="work-with-account-keys-and-connection-strings-for-an-azure-cosmos-account-using-azure-cli"></a>Trabalhe com chaves de conta e cordas de conexão para uma conta Azure Cosmos usando Azure CLI
[!INCLUDE[appliesto-all-apis](../../../includes/appliesto-all-apis.md)]

[!INCLUDE [azure-cli-prepare-your-environment.md](../../../../../includes/azure-cli-prepare-your-environment.md)]

- Este artigo requer a versão 2.9.1 ou posterior do Azure CLI. Se utilizar o Azure Cloud Shell, a versão mais recente já está instalada.

## <a name="sample-script"></a>Script de exemplo

Este guião demonstra quatro operações.

- Listar todas as chaves da conta
- Lista leia apenas chaves de conta
- Cadeias de ligação de lista
- Voltar a gerar chaves da conta

> [!NOTE]
> Esta amostra demonstra a utilização de uma conta API SQL (Core), mas as operações de chave de conta e de cadeias de conexão são idênticas em todas as APIs de base de dados em Cosmos DB.

[!code-azurecli-interactive[main](../../../../../cli_scripts/cosmosdb/common/keys.sh "Keys and connection string operations for Cosmos DB.")]

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
| [az cosmosdb create](/cli/azure/cosmosdb#az_cosmosdb_create) | Cria uma conta do Azure Cosmos DB. |
| [lista de chaves cosmosdb az](/cli/azure/cosmosdb/keys#az_cosmosdb_keys_list) | Lista as chaves de uma conta DB do Azure Cosmos. |
| [az cosmosdb lista-read-only-keys](/cli/azure/cosmosdb#az_cosmosdb_list_read_only_keys) | Lista as chaves de leitura para uma conta DB do Azure Cosmos. |
| [az cosmosdb list-connection-strings](/cli/azure/cosmosdb#az_cosmosdb_list_connection_strings) | Listar as cordas de ligação para uma conta DB Azure Cosmos. |
| [az cosmosdb regenerate-key](/cli/azure/cosmosdb#az_cosmosdb_regenerate-key) | Regenerar chaves para uma conta DB do Azure Cosmos. |
| [az group delete](/cli/azure/resource#az_resource_delete) | Elimina um grupo de recursos, incluindo todos os recursos aninhados. |

## <a name="next-steps"></a>Passos seguintes

Para mais informações sobre o Azure Cosmos DB CLI, consulte [a documentação do Azure Cosmos DB CLI](/cli/azure/cosmosdb).

Todas as amostras de scriptS Azure Cosmos DB CLI podem ser encontradas no [Repositório Azure Cosmos DB CLI GitHub](https://github.com/Azure-Samples/azure-cli-samples/tree/master/cosmosdb).
