---
title: Implementar recursos com API rest e modelo
description: Utilize o Azure Resource Manager e o Resource Manager REST API para mobilizar recursos para o Azure. Os recursos são definidos num modelo do Resource Manager.
ms.topic: conceptual
ms.date: 10/22/2020
ms.openlocfilehash: 90e50598176ddc0327a81df105740f58afd930bc
ms.sourcegitcommit: 32e0fedb80b5a5ed0d2336cea18c3ec3b5015ca1
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/30/2021
ms.locfileid: "105732572"
---
# <a name="deploy-resources-with-arm-templates-and-azure-resource-manager-rest-api"></a>Implementar recursos com modelos ARM e AZure Resource Manager REST API

Este artigo explica como usar o API rest gestor de recursos Azure com modelos Azure Resource Manager (modelos ARM) para implantar os seus recursos para o Azure.

Pode incluir o seu modelo no corpo de pedido ou ligar-se a um ficheiro. Ao utilizar um ficheiro, pode ser um ficheiro local ou um ficheiro externo que esteja disponível através de um URI. Quando o seu modelo estiver numa conta de armazenamento, pode restringir o acesso ao modelo e fornecer um sinal de assinatura de acesso partilhado (SAS) durante a implementação.

## <a name="deployment-scope"></a>Âmbito de implantação

Pode direcionar a sua implementação para um grupo de recursos, subscrição Azure, grupo de gestão ou inquilino. Dependendo do alcance da implantação, utiliza-se diferentes comandos.

- Para implementar num **grupo de recursos,** utilize [implementações - Crie](/rest/api/resources/resources/deployments/createorupdate). O pedido é enviado para:

  ```HTTP
  PUT https://management.azure.com/subscriptions/{subscriptionId}/resourcegroups/{resourceGroupName}/providers/Microsoft.Resources/deployments/{deploymentName}?api-version=2020-10-01
  ```

- Para implementar uma **subscrição**, utilize [implementações - Crie no âmbito de subscrição](/rest/api/resources/resources/deployments/createorupdateatsubscriptionscope). O pedido é enviado para:

  ```HTTP
  PUT https://management.azure.com/subscriptions/{subscriptionId}/providers/Microsoft.Resources/deployments/{deploymentName}?api-version=2020-10-01
  ```

  Para obter mais informações sobre as implementações do nível de subscrição, consulte [Criar grupos de recursos e recursos ao nível da subscrição.](deploy-to-subscription.md)

- Para implantar num **grupo de gestão,** utilize [implementações - Crie no Âmbito do Grupo de Gestão](/rest/api/resources/resources/deployments/createorupdateatmanagementgroupscope). O pedido é enviado para:

  ```HTTP
  PUT https://management.azure.com/providers/Microsoft.Management/managementGroups/{groupId}/providers/Microsoft.Resources/deployments/{deploymentName}?api-version=2020-10-01
  ```

  Para obter mais informações sobre as implementações de nível de grupo de gestão, consulte [Criar recursos ao nível do grupo de gestão.](deploy-to-management-group.md)

- Para implantar num **inquilino,** utilize [implementações - Crie ou atualize no âmbito do inquilino.](/rest/api/resources/resources/deployments/createorupdateattenantscope) O pedido é enviado para:

  ```HTTP
  PUT https://management.azure.com/providers/Microsoft.Resources/deployments/{deploymentName}?api-version=2020-10-01
  ```

  Para obter mais informações sobre as implementações de nível de inquilino, consulte [Criar recursos ao nível do arrendatário.](deploy-to-tenant.md)

Os exemplos deste artigo utilizam implementações de grupos de recursos.

## <a name="deploy-with-the-rest-api"></a>Implementar com a API REST

1. Desacorda [os parâmetros e cabeçalhos comuns,](/rest/api/azure/)incluindo fichas de autenticação.

1. Se estiver a implantar para um grupo de recursos que não existe, crie o grupo de recursos. Forneça o seu ID de subscrição, o nome do novo grupo de recursos e a localização que necessita para a sua solução. Para obter mais informações, consulte [Criar um grupo de recursos.](/rest/api/resources/resources/resourcegroups/createorupdate)

   ```HTTP
   PUT https://management.azure.com/subscriptions/<YourSubscriptionId>/resourcegroups/<YourResourceGroupName>?api-version=2020-06-01
   ```

   Com um corpo de pedido como:

   ```json
   {
    "location": "West US",
    "tags": {
      "tagname1": "tagvalue1"
    }
   }
   ```

1. Antes de implementar o seu modelo, pode visualizar as alterações que o modelo irá fazer para o seu ambiente. Utilize a [operação "e se"](template-deploy-what-if.md) para verificar se o modelo faz as alterações que espera. E se também valida o modelo para erros.

1. Para implementar um modelo, forneça o seu ID de subscrição, o nome do grupo de recursos, o nome da implementação no URI de pedido.

   ```HTTP
   PUT https://management.azure.com/subscriptions/<YourSubscriptionId>/resourcegroups/<YourResourceGroupName>/providers/Microsoft.Resources/deployments/<YourDeploymentName>?api-version=2020-10-01
   ```

   No corpo de pedido, forneça um link para o seu modelo e arquivo de parâmetros. Para obter mais informações sobre o ficheiro de parâmetros, veja [Criar ficheiro de parâmetros do Resource Manager](parameter-files.md).

   Note que o `mode` é definido para **Incremental**. Para executar uma implementação completa, definir `mode` para **completar**. Tenha cuidado ao utilizar o modo completo, pois pode eliminar inadvertidamente recursos que não estão no seu modelo.

   ```json
   {
    "properties": {
      "templateLink": {
        "uri": "http://mystorageaccount.blob.core.windows.net/templates/template.json",
        "contentVersion": "1.0.0.0"
      },
      "parametersLink": {
        "uri": "http://mystorageaccount.blob.core.windows.net/templates/parameters.json",
        "contentVersion": "1.0.0.0"
      },
      "mode": "Incremental"
    }
   }
   ```

    Se pretender registar o conteúdo da resposta, solicite conteúdo, ou ambos, inclua `debugSetting` no pedido.

   ```json
   {
    "properties": {
      "templateLink": {
        "uri": "http://mystorageaccount.blob.core.windows.net/templates/template.json",
        "contentVersion": "1.0.0.0"
      },
      "parametersLink": {
        "uri": "http://mystorageaccount.blob.core.windows.net/templates/parameters.json",
        "contentVersion": "1.0.0.0"
      },
      "mode": "Incremental",
      "debugSetting": {
        "detailLevel": "requestContent, responseContent"
      }
    }
   }
   ```

    Pode configurar a sua conta de armazenamento para utilizar um token de assinatura de acesso partilhado (SAS). Para mais informações, consulte [o Acesso do Delegado com uma assinatura de acesso partilhado.](/rest/api/storageservices/delegate-access-with-shared-access-signature)

    Se precisar de fornecer um valor sensível para um parâmetro (como uma palavra-passe), adicione esse valor a um cofre de chaves. Recupere o cofre da chave durante a colocação, como mostra o exemplo anterior. Para obter mais informações, consulte [use Azure Key Vault para passar o valor do parâmetro seguro durante a implementação](key-vault-parameter.md).

1. Em vez de ligar aos ficheiros para o modelo e parâmetros, pode incluí-los no corpo de pedido. O exemplo a seguir mostra o corpo de pedido com o modelo e o parâmetro inline:

   ```json
   {
      "properties": {
      "mode": "Incremental",
      "template": {
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
        "variables": {
          "storageAccountName": "[concat(uniquestring(resourceGroup().id), 'standardsa')]"
        },
        "resources": [
          {
            "type": "Microsoft.Storage/storageAccounts",
            "apiVersion": "2018-02-01",
            "name": "[variables('storageAccountName')]",
            "location": "[parameters('location')]",
            "sku": {
              "name": "[parameters('storageAccountType')]"
            },
            "kind": "StorageV2",
            "properties": {}
          }
        ],
        "outputs": {
          "storageAccountName": {
            "type": "string",
            "value": "[variables('storageAccountName')]"
          }
        }
      },
      "parameters": {
        "location": {
          "value": "eastus2"
        }
      }
    }
   }
   ```

1. Para obter o estado da implementação do modelo, use [Implementações - Obtenha](/rest/api/resources/resources/deployments/get).

   ```HTTP
   GET https://management.azure.com/subscriptions/{subscriptionId}/resourcegroups/{resourceGroupName}/providers/Microsoft.Resources/deployments/{deploymentName}?api-version=2020-10-01
   ```

## <a name="deployment-name"></a>Nome de implantação

Pode dar à sua implantação um nome como `ExampleDeployment` .

Sempre que executa uma implantação, uma entrada é adicionada ao histórico de implantação do grupo de recursos com o nome de implantação. Se executar outra implantação e lhe der o mesmo nome, a entrada anterior é substituída pela implementação atual. Se pretender manter entradas únicas no histórico de implantação, dê a cada implementação um nome único.

Para criar um nome único, pode atribuir um número aleatório. Ou adicionar um valor de data.

Se executar implementações simultâneas para o mesmo grupo de recursos com o mesmo nome de implantação, apenas a última implementação é concluída. Quaisquer implementações com o mesmo nome que não tenham terminado são substituídas pela última implantação. Por exemplo, se executar uma implantação com o nome `newStorage` de uma conta de armazenamento chamada , e ao mesmo tempo executar outra `storage1` implantação com o nome de uma `newStorage` conta de armazenamento chamada , implementa `storage2` apenas uma conta de armazenamento. A conta de armazenamento resultante é `storage2` nomeada.

No entanto, se executar uma implantação com o nome `newStorage` de uma conta de armazenamento chamada , e imediatamente após a sua `storage1` conclusão, executar outra implantação com o nome `newStorage` de uma conta de armazenamento chamada , `storage2` então tem duas contas de armazenamento. Um tem o nome `storage1` , e o outro chama-se `storage2` . Mas só tens uma entrada na história da implantação.

Quando especificar um nome único para cada implantação, pode executá-los simultaneamente sem conflitos. Se executar uma implantação com o nome `newStorage1` de uma conta de armazenamento chamada , e ao mesmo tempo executar outra `storage1` implantação com o nome de uma `newStorage2` conta de armazenamento , `storage2` então tem duas contas de armazenamento e duas entradas no histórico de implantação.

Para evitar conflitos com implementações simultâneas e para garantir entradas únicas no histórico de implantação, dê a cada implementação um nome único.

## <a name="next-steps"></a>Passos seguintes

- Para voltar a uma implementação bem sucedida quando tiver um erro, consulte [o Reversão do erro para uma implementação bem sucedida](rollback-on-error.md).
- Para especificar como lidar com os recursos que existem no grupo de recursos mas não estão definidos no modelo, consulte os [modos de implementação do Gestor de Recursos Azure](deployment-modes.md).
- Para aprender a lidar com operações assíncronas do REST, consulte [as operações de Azure assíncrona .](../management/async-operations.md)
- Para saber mais sobre os modelos, consulte [a estrutura e a sintaxe dos modelos ARM](template-syntax.md).
