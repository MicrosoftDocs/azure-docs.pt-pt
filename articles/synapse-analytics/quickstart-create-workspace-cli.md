---
title: 'Quickstart: Criar um espaço de trabalho synapse usando Azure CLI'
description: Crie um espaço de trabalho Azure Synapse utilizando o Azure CLI seguindo os passos deste guia.
services: synapse-analytics
author: alehall
ms.service: synapse-analytics
ms.topic: quickstart
ms.subservice: workspace
ms.date: 08/25/2020
ms.author: alehall
ms.reviewer: jrasnick
ms.openlocfilehash: 8a56b325dd5e1180b1229465965167241fab76a8
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/29/2021
ms.locfileid: "101676462"
---
# <a name="quickstart-create-an-azure-synapse-workspace-with-azure-cli"></a>Quickstart: Criar um espaço de trabalho sinapse Azure com Azure CLI

A CLI do Azure é a experiência da linha de comandos do Azure para a gestão de recursos do Azure. Pode utilizá-la no seu browser com o Azure Cloud Shell. Também pode instalá-la no macOS, Linux ou Windows e executá-la a partir da linha de comandos.

Neste arranque rápido, aprende-se a criar um espaço de trabalho synapse utilizando o CLI Azure.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Pré-requisitos

- Descarregue e instale [o JQ,](https://stedolan.github.io/jq/download/)um processador JSON de linha de comando leve e flexível
- [Conta de armazenamento de Azure Data Lake Gen2](../storage/common/storage-account-create.md)

    > [!IMPORTANT]
    > O espaço de trabalho Azure Synapse precisa de ser capaz de ler e escrever para a conta ADLS Gen2 selecionada. Além disso, para qualquer conta de armazenamento que ligue como conta de armazenamento primário, deve ter ativado o **espaço hierárquico** na criação da conta de armazenamento, conforme descrito na página [Criar um Accout de Armazenamento.](../storage/common/storage-account-create.md?tabs=azure-portal#create-a-storage-account) 

[!INCLUDE [azure-cli-prepare-your-environment.md](../../includes/azure-cli-prepare-your-environment-no-header.md)]

## <a name="create-an-azure-synapse-workspace-using-the-azure-cli"></a>Criar um espaço de trabalho Azure Synapse utilizando o Azure CLI

1. Definir variáveis ambientais necessárias para criar recursos para o espaço de trabalho Azure Synapse.

    | Nome da Variável de Ambiente | Description |
    |---|---|---|
    |StorageAccountName| Nome da sua conta de armazenamento ADLS Gen2 existente.|
    |StorageAccountResourceGroup| Nome do seu grupo de recursos de conta de armazenamento ADLS Gen2 existente. |
    |Nome de FileShare| Nome do seu sistema de ficheiros de armazenamento existente.|
    |Grupo SynapseResource| Escolha um novo nome para o seu grupo de recursos Azure Synapse. |
    |Region| Escolha uma das [regiões de Azure.](https://azure.microsoft.com/global-infrastructure/geographies/#overview) |
    |SinapseWorkspaceName| Escolha um nome único para o seu novo espaço de trabalho Azure Synapse. |
    |SqlUser| Escolha um valor para um novo nome de utilizador.|
    |SqlPassword| Escolha uma senha segura.|
    |||

1. Crie um grupo de recursos como recipiente para o seu espaço de trabalho Azure Synapse:
    ```azurecli
    az group create --name $SynapseResourceGroup --location $Region
    ```

1. Criar um espaço de trabalho Azure Synapse:
    ```azurecli
    az synapse workspace create \
      --name $SynapseWorkspaceName \
      --resource-group $SynapseResourceGroup \
      --storage-account $StorageAccountName \
      --file-system $FileShareName \
      --sql-admin-login-user $SqlUser \
      --sql-admin-login-password $SqlPassword \
      --location $Region
    ```

1. Obtenha Web e Dev URL para O espaço de trabalho Azure Synapse:
    ```azurecli
    WorkspaceWeb=$(az synapse workspace show --name $SynapseWorkspaceName --resource-group $SynapseResourceGroup | jq -r '.connectivityEndpoints | .web')

    WorkspaceDev=$(az synapse workspace show --name $SynapseWorkspaceName --resource-group $SynapseResourceGroup | jq -r '.connectivityEndpoints | .dev')
    ```

1. Crie uma Regra de Firewall para permitir o seu acesso ao Espaço de Trabalho Azure Synapse a partir da sua máquina:

    ```azurecli
    ClientIP=$(curl -sb -H "Accept: application/json" "$WorkspaceDev" | jq -r '.message')
    ClientIP=${ClientIP##'Client Ip address : '}
    echo "Creating a firewall rule to enable access for IP address: $ClientIP"

    az synapse workspace firewall-rule create --end-ip-address $ClientIP --start-ip-address $ClientIP --name "Allow Client IP" --resource-group $SynapseResourceGroup --workspace-name $SynapseWorkspaceName
    ```

1. Abra o endereço WEB URL do Espaço de Trabalho Azure Synapse armazenado em variável ambiente `WorkspaceWeb` para aceder ao seu espaço de trabalho:

    ```azurecli
    echo "Open your Azure Synapse Workspace Web URL in the browser: $WorkspaceWeb"
    ```
    
    [![Azure Synapse workspace web ](media/quickstart-create-synapse-workspace-cli/create-workspace-cli-1.png)](media/quickstart-create-synapse-workspace-cli/create-workspace-cli-1.png#lightbox)


## <a name="clean-up-resources"></a>Limpar os recursos

Siga os passos abaixo para eliminar o espaço de trabalho Azure Synapse.
> [!WARNING]
> A eliminação de um espaço de trabalho Azure Synapse removerá os motores de análise e os dados armazenados na base de dados dos pools SQL contidos e metadados do espaço de trabalho. Deixará de ser possível ligar-se aos pontos finais SQL ou Apache Spark. Todos os artefactos de código serão eliminados (consultas, cadernos, definições de emprego e oleodutos).
>
> A eliminação do espaço de trabalho **não** afetará os dados da Data Lake Store Gen2 ligada ao espaço de trabalho.

Se pretender eliminar o espaço de trabalho Azure Synapse, complete o seguinte comando:

```azurecli
az synapse workspace delete --name $SynapseWorkspaceName --resource-group $SynapseResourceGroup
```

## <a name="next-steps"></a>Passos seguintes

Em seguida, você pode [criar piscinas SQL](quickstart-create-sql-pool-studio.md) ou [criar piscinas Apache Spark](quickstart-create-apache-spark-pool-studio.md) para começar a analisar e explorar seus dados.