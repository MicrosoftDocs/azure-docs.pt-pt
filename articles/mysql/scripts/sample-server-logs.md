---
title: Script CLI - Descarregue registos de consulta lenta - Base de Dados Azure para MySQL
description: Este script de exemplo da CLI mostra como ativar e transferir os registos de um servidor da Base de Dados do Azure para MySQL.
author: savjani
ms.author: pariks
ms.service: mysql
ms.devlang: azurecli
ms.topic: sample
ms.custom: mvc, devx-track-azurecli
ms.date: 12/02/2019
ms.openlocfilehash: 15d22ea3e0d16d377847ba85acfa41f5ae1043a6
ms.sourcegitcommit: 4b0e424f5aa8a11daf0eec32456854542a2f5df0
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/20/2021
ms.locfileid: "107791702"
---
# <a name="enable-and-download-server-slow-query-logs-of-an-azure-database-for-mysql-server-using-azure-cli"></a>Ativar e transferir registos de consulta lenta de um servidor da Base de Dados do Azure para MySQL com a CLI do Azure
Este script de exemplo da CLI ativa e transfere os registos de consulta lenta de um único servidor da Base de Dados do Azure para MySQL.

[!INCLUDE [azure-cli-prepare-your-environment.md](../../../includes/azure-cli-prepare-your-environment.md)]

- Este artigo requer a versão 2.0 ou posterior do Azure CLI. Se utilizar o Azure Cloud Shell, a versão mais recente já está instalada. 

## <a name="sample-script"></a>Script de exemplo
Neste script de exemplo, edite as linhas realçadas para atualizar o nome de utilizador administrador e a palavra-passe com os seus. Substitua a &lt; log_file_name &gt; nos `az monitor` comandos pelo nome do ficheiro de registo do seu próprio servidor.
[!code-azurecli-interactive[main](../../../cli_scripts/mysql/server-logs/server-logs.sh?highlight=15-16 "Manipulate with server logs.")]

## <a name="clean-up-deployment"></a>Limpar a implementação
Utilize o comando seguinte para remover o grupo de recursos e todos os recursos associados ao mesmo, depois de executar o script. 
[!code-azurecli-interactive[main](../../../cli_scripts/mysql/server-logs/delete-mysql.sh  "Delete the resource group.")]

## <a name="script-explanation"></a>Explicação do script
Este script utiliza os comandos descritos na tabela seguinte:

| **Comando** | **Notas** |
|---|---|
| [az group create](/cli/azure/group#az_group_create) | Cria um grupo de recursos no qual todos os recursos são armazenados. |
| [az mysql server create](/cli/azure/mysql/server#az_mysql_server_create) | Cria um servidor MySQL que aloja as bases de dados. |
| [az mysql server configuration list](/cli/azure/mysql/server/configuration#az_mysql_server_configuration_list) | Liste os valores de configuração para um servidor. |
| [az mysql server configuration set](/cli/azure/mysql/server/configuration#az_mysql_server_configuration_set) | Atualize a configuração de um servidor. |
| [az mysql server-logs list](/cli/azure/mysql/server-logs#az_mysql_server_logs_list) | Liste os ficheiros de registo para um servidor. |
| [az mysql server-logs download](/cli/azure/mysql/server-logs#az_mysql_server_logs_download) | Descarregue ficheiros de registo. |
| [az group delete](/cli/azure/group#az_group_delete) | Elimina um grupo de recursos, incluindo todos os recursos aninhados. |

## <a name="next-steps"></a>Passos seguintes
- Leia mais informações sobre a CLI do Azure: [Documentação da CLI do Azure](/cli/azure).
- Experimente scripts adicionais: [Exemplos da CLI do Azure para a Base de Dados do Azure para MySQL](../sample-scripts-azure-cli.md)
