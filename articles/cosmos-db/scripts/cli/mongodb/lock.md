---
title: Criar bloqueio de recursos para uma base de dados e recolha para MongoDB API para Azure Cosmos DB
description: Criar bloqueio de recursos para uma base de dados e recolha para MongoDB API para Azure Cosmos DB
author: markjbrown
ms.author: mjbrown
ms.service: cosmos-db
ms.subservice: cosmosdb-mongo
ms.topic: sample
ms.date: 07/29/2020
ms.openlocfilehash: ee298b33736ae25b365cc54ee1bb5ec9f38bfb62
ms.sourcegitcommit: 4b0e424f5aa8a11daf0eec32456854542a2f5df0
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/20/2021
ms.locfileid: "107763616"
---
# <a name="create-a-resource-lock-for-azure-cosmos-dbs-api-for-mongodb-using-azure-cli"></a>Crie um bloqueio de recursos para a API da Azure Cosmos DB para a MongoDB usando o Azure CLI
[!INCLUDE[appliesto-mongodb-api](../../../includes/appliesto-mongodb-api.md)]

[!INCLUDE [azure-cli-prepare-your-environment.md](../../../../../includes/azure-cli-prepare-your-environment.md)]

- Este artigo requer a versão 2.9.1 ou posterior do Azure CLI. Se utilizar o Azure Cloud Shell, a versão mais recente já está instalada.

> [!IMPORTANT]
> Os bloqueios de recursos não funcionam para alterações efetuadas pelos utilizadores que se ligam utilizando qualquer MongoDB SDK, Mongoshell, quaisquer ferramentas ou o Portal Azure, a menos que a conta Cosmos DB seja bloqueada pela primeira vez com a `disableKeyBasedMetadataWriteAccess` propriedade ativada. Para saber mais sobre como ativar esta propriedade ver, [Prevenindo alterações de SDKs.](../../../role-based-access-control.md#prevent-sdk-changes)

## <a name="sample-script"></a>Script de exemplo

[!code-azurecli-interactive[main](../../../../../cli_scripts/cosmosdb/mongodb/lock.sh "Create a resource lock for an Azure Cosmos DB MongoDB API database and collection.")]

## <a name="script-explanation"></a>Explicação do script

Este script utiliza os seguintes comandos. Cada comando na tabela liga à documentação específica do comando.

| Comando | Notas |
|---|---|
| [bloqueio az criar](/cli/azure/lock#az_lock_create) | Cria uma fechadura. |
| [lista de bloqueio az](/cli/azure/lock#az_lock_list) | Lista informação de bloqueio. |
| [az lock show](/cli/azure/lock#az_lock_show) | Mostrar propriedades de uma fechadura. |
| [bloqueio az eliminar](/cli/azure/lock#az_lock_delete) | Apaga uma fechadura. |

## <a name="next-steps"></a>Passos seguintes

- [Bloquear recursos para prevenir alterações inesperadas](../../../../azure-resource-manager/management/lock-resources.md)

- [Documentação do Azure Cosmos DB CLI](/cli/azure/cosmosdb).

- [Azure Cosmos DB CLI GitHub Repository](https://github.com/Azure-Samples/azure-cli-samples/tree/master/cosmosdb).
