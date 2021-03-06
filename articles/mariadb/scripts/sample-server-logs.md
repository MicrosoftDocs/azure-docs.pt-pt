---
title: Script CLI - Descarregue registos de consulta lenta - Base de Dados Azure para MariaDB
description: Esta amostra O script Azure CLI mostra como ativar e descarregar os registos de consulta lenta de uma Base de Dados Azure para servidor MariaDB.
author: savjani
ms.author: pariks
ms.service: mariadb
ms.devlang: azurecli
ms.topic: sample
ms.custom: mvc, devx-track-azurecli
ms.date: 12/02/2019
ms.openlocfilehash: 42b1806d3aa1235dfd976dd5700204aa0b9e4700
ms.sourcegitcommit: 4b0e424f5aa8a11daf0eec32456854542a2f5df0
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/20/2021
ms.locfileid: "107785564"
---
# <a name="enable-and-download-server-slow-query-logs-of-an-azure-database-for-mariadb-server-using-azure-cli"></a>Ative e descarregue registos de consulta lenta do servidor de uma Base de Dados Azure para servidor MariaDB utilizando O Azure CLI
Esta amostra de script CLI permite e descarrega os registos de consulta lenta de uma única Base de Dados Azure para o servidor MariaDB.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [azure-cli-prepare-your-environment.md](../../../includes/azure-cli-prepare-your-environment.md)]

- Este artigo requer a versão 2.0 ou posterior do Azure CLI. Se utilizar o Azure Cloud Shell, a versão mais recente já está instalada.

## <a name="sample-script"></a>Script de exemplo
Neste script de exemplo, edite as linhas realçadas para atualizar o nome de utilizador administrador e a palavra-passe com os seus. Substitua a &lt; log_file_name &gt; nos `az monitor` comandos pelo nome do ficheiro de registo do seu próprio servidor.
[!code-azurecli-interactive[main](../../../cli_scripts/mariadb/server-logs/server-logs.sh?highlight=15-16 "Manipulate with server logs.")]

## <a name="clean-up-deployment"></a>Limpar a implementação
Utilize o comando seguinte para remover o grupo de recursos e todos os recursos associados ao mesmo, depois de executar o script. 
[!code-azurecli-interactive[main](../../../cli_scripts/mariadb/server-logs/delete-mariadb.sh  "Delete the resource group.")]

## <a name="script-explanation"></a>Explicação do script
Este script utiliza os comandos descritos na tabela seguinte:

| **Comando** | **Notas** |
|---|---|
| [az group create](/cli/azure/group#az_group_create) | Cria um grupo de recursos no qual todos os recursos são armazenados. |
| [servidor mariadb az criar](/cli/azure/mariadb/server#az_mariadb_server_create) | Cria um servidor MariaDB que acolhe as bases de dados. |
| [lista de configuração do servidor mariadb az](/cli/azure/mariadb/server/configuration#az_mariadb_server_configuration_list) | Liste os valores de configuração para um servidor. |
| [conjunto de configuração do servidor mariadb az](/cli/azure/mariadb/server/configuration#az_mariadb_server_configuration_set) | Atualize a configuração de um servidor. |
| [lista de registos de servidores az mariadb](/cli/azure/mariadb/server-logs#az_mariadb_server_logs_list) | Liste os ficheiros de registo para um servidor. |
| [download de servidores mariadb az](/cli/azure/mariadb/server-logs#az_mariadb_server_logs_download) | Descarregue ficheiros de registo. |
| [az group delete](/cli/azure/group#az_group_delete) | Elimina um grupo de recursos, incluindo todos os recursos aninhados. |

## <a name="next-steps"></a>Passos seguintes
- Leia mais informações sobre a CLI do Azure: [Documentação da CLI do Azure](/cli/azure).
- Experimente scripts adicionais: [amostras de Azure CLI para Azure Database for MariaDB](../sample-scripts-azure-cli.md)
