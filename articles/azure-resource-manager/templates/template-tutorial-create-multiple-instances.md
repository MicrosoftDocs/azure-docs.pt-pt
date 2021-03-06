---
title: Criar várias instâncias de recurso
description: Aprenda a criar um modelo de Gestor de Recursos Azure (modelo ARM) para criar múltiplos casos de recursos Azure.
author: mumian
ms.date: 04/23/2020
ms.topic: tutorial
ms.author: jgao
ms.custom: ''
ms.openlocfilehash: e669e27547633639a88674ffee499fb1d84facdf
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/29/2021
ms.locfileid: "97673958"
---
# <a name="tutorial-create-multiple-resource-instances-with-arm-templates"></a>Tutorial: Criar várias instâncias de recursos com modelos do ARM

Aprenda a iterar no seu modelo de Gestor de Recursos Azure (modelo ARM) para criar várias instâncias de um recurso Azure. Neste tutorial, modifica um modelo para criar três instâncias de contas de armazenamento.

![Azure Resource Manager cria diagrama de múltiplas instâncias](./media/template-tutorial-create-multiple-instances/resource-manager-template-create-multiple-instances-diagram.png)

Este tutorial abrange as seguintes tarefas:

> [!div class="checklist"]
> * Abrir um modelo de Início Rápido
> * Editar o modelo
> * Implementar o modelo

Se não tiver uma subscrição do Azure, [crie uma conta gratuita](https://azure.microsoft.com/free/) antes de começar.

Para um módulo Microsoft Learn que cubra a cópia de recursos, consulte [Gerir implementações complexas em nuvem utilizando funcionalidades avançadas do modelo ARM](/learn/modules/manage-deployments-advanced-arm-template-features/).

## <a name="prerequisites"></a>Pré-requisitos

Para concluir este artigo, precisa de:

* Visual Studio Code com extensão Ferramentas do Resource Manager. Consulte [Quickstart: Crie modelos ARM com Código de Estúdio Visual](quickstart-create-templates-use-visual-studio-code.md).

## <a name="open-a-quickstart-template"></a>Abrir um modelo de Início Rápido

[Azure Quickstart Templates](https://azure.microsoft.com/resources/templates/) é um repositório para modelos ARM. Em vez de criar um modelo do zero, pode encontrar um modelo de exemplo e personalizá-lo. O modelo utilizado neste início rápido chama-se [Criar uma conta de armazenamento padrão](https://azure.microsoft.com/resources/templates/101-storage-account-create/). O modelo define um recurso de conta de Armazenamento do Azure.

1. A partir do Código do Estúdio Visual, selecione Ficheiro Aberto **de**  >  **Ficheiros**.
1. em **Nome de ficheiro**, cole o seguinte URL:

    ```url
    https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-storage-account-create/azuredeploy.json
    ```

1. Selecione **Abrir** para abrir o ficheiro.
1. Há um `Microsoft.Storage/storageAccounts` recurso definido no modelo. Compare o modelo à [referência do modelo](/azure/templates/Microsoft.Storage/storageAccounts). É útil obter alguma compreensão básica do modelo antes de personalizá-lo.
1. Selecione   >  **'Guardar ficheiros' para** guardar o ficheiro à medida _queazuredeploy.jsno_ computador local.

## <a name="edit-the-template"></a>Editar o modelo

O exemplo existente cria uma conta de armazenamento. Personaliza o modelo para criar três contas de armazenamento.

No Visual Studio Code, efetue as seguintes quatro alterações:

![O Azure Resource Manager cria várias instâncias](./media/template-tutorial-create-multiple-instances/resource-manager-template-create-multiple-instances.png)

1. Adicione um elemento `copy` à definição do recurso de conta de armazenamento. No `copy` elemento, especifique o número de iterações e uma variável para este ciclo. O valor tem de ser um número inteiro positivo e não pode ser mais de 800.
2. A função `copyIndex()` devolve a iteração atual no ciclo. Utilize o índice como o prefixo do nome. `copyIndex()` é baseado em zero. Para compensar o valor do índice, pode passar um valor na `copyIndex()` função. Por exemplo, `copyIndex(1)`.
3. Apague o `variables` elemento, porque já não é usado.
4. Apague o `outputs` elemento. Já não é necessário.

O modelo completo assemelha-se a:

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "storageAccountType": {
      "type": "string",
      "defaultValue": "Standard_LRS",
      "allowedValues": [
        "Standard_LRS",
        "Standard_GRS",
        "Standard_ZRS",
        "Premium_LRS"
      ],
      "metadata": {
        "description": "Storage Account type"
      }
    },
    "location": {
      "type": "string",
      "defaultValue": "[resourceGroup().location]",
      "metadata": {
        "description": "Location for all resources."
      }
    }
  },
  "resources": [
    {
      "type": "Microsoft.Storage/storageAccounts",
      "apiVersion": "2019-04-01",
      "name": "[concat(copyIndex(),'storage', uniqueString(resourceGroup().id))]",
      "location": "[parameters('location')]",
      "sku": {
        "name": "[parameters('storageAccountType')]"
      },
      "kind": "StorageV2",
      "copy": {
        "name": "storagecopy",
        "count": 3
      },
      "properties": {}
    }
  ]
}
```

Para obter mais informações sobre a criação de múltiplas instâncias, consulte [a iteração de recursos nos modelos ARM](./copy-resources.md)

## <a name="deploy-the-template"></a>Implementar o modelo

1. Inscreva-se na [Azure Cloud Shell](https://shell.azure.com)

1. Escolha o seu ambiente preferido selecionando **PowerShell** ou **Bash** (para CLI) no canto superior esquerdo. É necessário reiniciar o Shell quando mudar.

    ![Arquivo de upload do portal Azure Cloud Shell](./media/template-tutorial-use-template-reference/azure-portal-cloud-shell-upload-file.png)

1. Selecione **Carregar/transferir ficheiros** e, em seguida, selecione **Carregar**. Veja a captura de ecrã anterior. Selecione o ficheiro que guardou na secção anterior. Depois de carregar o ficheiro, pode utilizar o `ls` comando e o comando para verificar se o ficheiro foi carregado com `cat` sucesso.

1. A partir da Cloud Shell, executar os seguintes comandos. Selecione o separador para mostrar o código do PowerShell ou o código da CLI.

    # <a name="cli"></a>[CLI](#tab/CLI)

    ```azurecli
    echo "Enter a project name that is used to generate resource group name:" &&
    read projectName &&
    echo "Enter the location (i.e. centralus):" &&
    read location &&
    resourceGroupName="${projectName}rg" &&
    az group create --name $resourceGroupName --location "$location" &&
    az deployment group create --resource-group $resourceGroupName --template-file "$HOME/azuredeploy.json"
    ```

    # <a name="powershell"></a>[PowerShell](#tab/PowerShell)

    ```azurepowershell
    $projectName = Read-Host -Prompt "Enter a project name that is used to generate resource group name"
    $location = Read-Host -Prompt "Enter the location (i.e. centralus)"
    $resourceGroupName = "${projectName}rg"

    New-AzResourceGroup -Name $resourceGroupName -Location "$location"
    New-AzResourceGroupDeployment -ResourceGroupName $resourceGroupName -TemplateFile "$HOME/azuredeploy.json"
    ```

    ---

Após uma implementação bem sucedida do modelo, pode exibir as três contas de armazenamento criadas no grupo de recursos especificado. Compare os nomes de contas de armazenamento com a definição de nome no modelo.

# <a name="cli"></a>[CLI](#tab/azure-cli)

```azurecli
echo "Enter a project name that is used to generate resource group name:" &&
read projectName &&
resourceGroupName="${projectName}rg" &&
az storage account list --resource-group $resourceGroupName &&
echo "Press [ENTER] to continue ..."
```

# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

```azurepowershell
$projectName = Read-Host -Prompt "Enter a project name that is used to generate resource group name"
$resourceGroupName = "${projectName}rg"

Get-AzStorageAccount -ResourceGroupName $resourceGroupName
Write-Host "Press [ENTER] to continue ..."
```

---

## <a name="clean-up-resources"></a>Limpar recursos

Quando os recursos do Azure já não forem necessários, limpe os recursos implementados ao eliminar o grupo de recursos.

1. A partir do portal Azure, selecione Grupo de **Recursos** do menu esquerdo.
2. Introduza o nome do grupo de recursos no campo **Filtrar por nome**.
3. Selecione o nome do grupo de recursos.  Verá um total de três recursos no grupo de recursos.
4. **Selecione Eliminar o grupo** de recursos do menu superior.

## <a name="next-steps"></a>Passos seguintes

Neste tutorial, aprendeu a criar várias instâncias de contas de armazenamento. No próximo tutorial, vai desenvolver um modelo com vários recursos e vários tipos de recurso. Alguns dos recursos têm recursos dependentes.

> [!div class="nextstepaction"]
> [Criar recursos dependentes](./template-tutorial-create-templates-with-dependent-resources.md)
