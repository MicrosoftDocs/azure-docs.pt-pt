---
title: 'CLI: Criar uma cópia de segurança programada'
description: Saiba como utilizar o Azure CLI para automatizar a implementação e gestão da sua aplicação De Serviço de Aplicações. Esta amostra mostra como criar uma cópia de segurança programada para uma aplicação.
author: msangapu-msft
tags: azure-service-management
ms.devlang: azurecli
ms.topic: sample
ms.date: 12/11/2017
ms.author: msangapu
ms.reviewer: cephalin
ms.custom: mvc, seodec18, devx-track-azurecli
ms.openlocfilehash: db1d0558f93b203af1605663533847d32afbcffb
ms.sourcegitcommit: 4b0e424f5aa8a11daf0eec32456854542a2f5df0
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/20/2021
ms.locfileid: "107782559"
---
# <a name="create-a-scheduled-backup-for-an-app-service-app-using-cli"></a>Crie uma cópia de segurança programada para uma aplicação do Serviço de Aplicações utilizando o CLI

Este script de amostra cria uma aplicação no Serviço de Aplicações com os seus recursos relacionados e, em seguida, cria uma cópia de segurança programada para o mesmo. 

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Se optar por instalar e utilizar a CLI localmente, precisa da versão 2.0 ou posterior da CLI do Azure. Para localizar a versão, execute `az --version`. Se precisar de instalar ou atualizar, veja [Instalar a CLI do Azure]( /cli/azure/install-azure-cli). 

## <a name="sample-script"></a>Script de exemplo

[!code-azurecli-interactive[main](../../../cli_scripts/app-service/backup-scheduled/backup-scheduled.sh?highlight=3-7 "Create a scheduled backup for an app")]

[!INCLUDE [cli-script-clean-up](../../../includes/cli-script-clean-up.md)]

## <a name="script-explanation"></a>Explicação do script

Este script utiliza os seguintes comandos. Cada comando na tabela liga à documentação específica do comando.

| Comando | Notas |
|---|---|
| [`az group create`](/cli/azure/group#az_group_create) | Cria um grupo de recursos no qual todos os recursos são armazenados. |
| [`az storage account create`](/cli/azure/storage/account#az_storage_account_create) | Cria uma conta de armazenamento. |
| [`az storage container create`](/cli/azure/storage/container#az_storage_container_create) | Cria um contentor de armazenamento do Azure. |
| [`az storage container generate-sas`](/cli/azure/storage/container#az_storage_container_generate_sas) | Gera um token SAS para um contentor de armazenamento do Azure.  |
| [`az appservice plan create`](/cli/azure/appservice/plan#az_appservice_plan_create) | Cria um plano do Serviço de Aplicações. |
| [`az webapp create`](/cli/azure/webapp#az_webapp_create) | Cria uma aplicação de Serviço de Aplicações. |
| [`az webapp config backup update`](/cli/azure/webapp/config/backup#az_webapp_config_backup_update) | Configura um novo horário de backup para uma aplicação do Serviço de Aplicações. |
| [`az webapp config backup show`](/cli/azure/webapp/config/backup#az_webapp_config_backup_show) | Mostra a agenda de backup para uma aplicação do Serviço de Aplicações. |
| [`az webapp config backup list`](/cli/azure/webapp/config/backup#az_webapp_config_backup_list) | Obtém uma lista de backups para uma aplicação do Serviço de Aplicações. |

## <a name="next-steps"></a>Passos seguintes

Para obter mais informações sobre a CLI do Azure, veja [Documentação da CLI do Azure](/cli/azure).

Pode ver exemplos do script da CLI do Serviço de Aplicações adicionais na [documentação do Serviço de Aplicações do Azure](../samples-cli.md).
