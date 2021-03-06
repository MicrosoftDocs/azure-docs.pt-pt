---
title: Crie uma loja de configuração de aplicativos Azure utilizando o modelo do Gestor de Recursos Azure (modelo ARM)
titleSuffix: Azure App Configuration
description: Aprenda a criar uma loja de configuração de aplicativos Azure utilizando o modelo Azure Resource Manager (modelo ARM).
author: GrantMeStrength
ms.author: jken
ms.date: 10/16/2020
ms.service: azure-app-configuration
ms.topic: quickstart
ms.custom: subject-armqs, devx-track-azurepowershell
ms.openlocfilehash: 92ca80a6c807394c45be8f0187c7add736ba83ce
ms.sourcegitcommit: 3c460886f53a84ae104d8a09d94acb3444a23cdc
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/21/2021
ms.locfileid: "107831785"
---
# <a name="quickstart-create-an-azure-app-configuration-store-by-using-an-arm-template"></a>Quickstart: Criar uma loja de configuração de aplicativos Azure usando um modelo ARM

Este quickstart descreve como:

- Implemente uma loja de configuração de aplicativos utilizando um modelo de Gestor de Recursos Azure (modelo ARM).
- Crie valores-chave numa loja de configuração de aplicativos utilizando o modelo ARM.
- Leia os valores-chave numa loja de configuração de aplicações a partir do modelo ARM.

[!INCLUDE [About Azure Resource Manager](../../includes/resource-manager-quickstart-introduction.md)]

Se o seu ambiente cumpre os pré-requisitos e se está familiarizado com a utilização de modelos ARM, selecione o botão **Implementar no Azure**. O modelo será aberto no portal do Azure.

[![Implementar no Azure](../media/template-deployments/deploy-to-azure.svg)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-app-configuration-store-kv%2Fazuredeploy.json)

## <a name="prerequisites"></a>Pré-requisitos

Se não tiver uma subscrição do Azure, crie uma [conta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de começar.

## <a name="review-the-template"></a>Rever o modelo

O modelo utilizado neste início rápido pertence aos [Modelos de Início Rápido do Azure](https://azure.microsoft.com/resources/templates/101-app-configuration-store-kv/). Cria uma nova loja de Configuração de Aplicações com dois valores-chave no interior. Em seguida, utiliza a `reference` função para desau o valor dos dois recursos de valor-chave. A leitura do valor da chave desta forma permite que seja usada em outros lugares do modelo.

O quickstart utiliza o `copy` elemento para criar múltiplas instâncias de recurso de valor-chave. Para saber mais sobre o `copy` elemento, consulte [a iteração de recursos nos modelos ARM](../azure-resource-manager/templates/copy-resources.md).

> [!IMPORTANT]
> Este modelo requer a versão do fornecedor de recursos de configuração de aplicação `2020-07-01-preview` ou posterior. Esta versão utiliza a `reference` função para ler valores-chave. A `listKeyValue` função que foi utilizada para ler valores-chave na versão anterior não está disponível a partir da versão `2020-07-01-preview` .

:::code language="json" source="~/quickstart-templates/101-app-configuration-store-kv/azuredeploy.json":::

Dois recursos Azure são definidos no modelo:

- [Microsoft.AppConfiguration/configurationStores:](/azure/templates/microsoft.appconfiguration/2020-07-01-preview/configurationstores)criar uma loja de configuração de aplicações.
- [Microsoft.AppConfiguration/configurationStores/keyValues:](/azure/templates/microsoft.appconfiguration/2020-07-01-preview/configurationstores/keyvalues)crie um valor-chave dentro da loja de Configuração de Aplicações.

> [!TIP]
> O `keyValues` nome do recurso é uma combinação de chave e etiqueta. A chave e o rótulo são acompanhados pelo `$` delimiter. A etiqueta é opcional. No exemplo acima, o `keyValues` recurso com nome cria um `myKey` valor-chave sem rótulo.
>
> A codificação por percentativa, também conhecida como codificação de URL, permite que as chaves ou etiquetas incluam caracteres que não são permitidos em nomes de recursos de modelo ARM. `%` também não é um personagem permitido, pelo que `~` é usado no seu lugar. Para codificar corretamente um nome, siga estes passos:
>
> 1. Aplicar codificação de URL
> 2. Substituir `~` por `~7E`
> 3. Substituir `%` por `~`
>
> Por exemplo, para criar um par de valores-chave com nome chave `AppName:DbEndpoint` e nome `Test` de etiqueta, o nome do recurso deve ser `AppName~3ADbEndpoint$Test` .

> [!NOTE]
> A Configuração da Aplicação permite o acesso de dados de valor chave a um [link privado](concept-private-endpoint.md) a partir da sua rede virtual. Por predefinição, quando a funcionalidade está ativada, todos os pedidos para os dados de Configuração da aplicação através da rede pública são negados. Como o modelo ARM funciona fora da sua rede virtual, o acesso de dados a partir de um modelo ARM não é permitido. Para permitir o acesso de dados a partir de um modelo ARM quando um link privado é utilizado, pode ativar o acesso à rede pública utilizando o seguinte comando Azure CLI. É importante considerar as implicações de segurança de permitir o acesso à rede pública neste cenário.
>
> ```azurecli-interactive
> az appconfig update -g MyResourceGroup -n MyAppConfiguration --enable-public-network true
> ```

## <a name="deploy-the-template"></a>Implementar o modelo

Selecione a imagem seguinte para iniciar sessão no Azure e abrir um modelo. O modelo cria uma loja de configuração de aplicativos com dois valores-chave no interior.

[![Implementar no Azure](../media/template-deployments/deploy-to-azure.svg)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-app-configuration-store-kv%2Fazuredeploy.json)

Também pode implantar o modelo utilizando o seguinte cmdlet PowerShell. Os valores-chave estarão na saída da consola PowerShell.

```azurepowershell-interactive
$projectName = Read-Host -Prompt "Enter a project name that is used for generating resource names"
$location = Read-Host -Prompt "Enter the location (i.e. centralus)"
$templateUri = "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-app-configuration-store-kv/azuredeploy.json"

$resourceGroupName = "${projectName}rg"

New-AzResourceGroup -Name $resourceGroupName -Location "$location"
New-AzResourceGroupDeployment -ResourceGroupName $resourceGroupName -TemplateUri $templateUri

Read-Host -Prompt "Press [ENTER] to continue ..."
```

## <a name="review-deployed-resources"></a>Revisão dos recursos implantados

1. Inicie sessão no [portal do Azure](https://portal.azure.com).
1. Na caixa de pesquisa do portal Azure, **escreva configuração de aplicação**. Selecione configuração de **aplicação** da lista.
1. Selecione o recurso de configuração de aplicação recém-criado.
1. Em **Operações**, clique no **explorador de configuração**.
1. Verifique se existem dois valores-chave.

## <a name="clean-up-resources"></a>Limpar os recursos

Quando já não for necessário, elimine o grupo de recursos, a loja de Configuração de Aplicações e todos os recursos relacionados. Se estiver a planear utilizar a loja de Configuração de Aplicações no futuro, pode não a excluir. Se não continuar a utilizar esta loja, elimine todos os recursos criados por este arranque rápido executando o seguinte cmdlet:

```azurepowershell-interactive
$resourceGroupName = Read-Host -Prompt "Enter the Resource Group name"
Remove-AzResourceGroup -Name $resourceGroupName
Write-Host "Press [ENTER] to continue..."
```

## <a name="next-steps"></a>Passos seguintes

Para saber mais sobre a adição de bandeira de funcionalidade e referência do Key Vault a uma loja de configuração de aplicações, consulte abaixo exemplos do modelo ARM.

- [101-app-configuração-loja-ff](https://github.com/Azure/azure-quickstart-templates/tree/master/101-app-configuration-store-ff)
- [101-app-configuração-loja-keyvaultref](https://github.com/Azure/azure-quickstart-templates/tree/master/101-app-configuration-store-keyvaultref)
