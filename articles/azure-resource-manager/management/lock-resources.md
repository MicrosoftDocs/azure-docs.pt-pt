---
title: Bloquear recursos para evitar alterações
description: Impedir que os utilizadores atualem ou apaguem os recursos do Azure aplicando um bloqueio para todos os utilizadores e funções.
ms.topic: conceptual
ms.date: 04/07/2021
ms.custom: devx-track-azurecli
ms.openlocfilehash: 71637318a60e66bf5000de2f564d740cc101cc60
ms.sourcegitcommit: 4b0e424f5aa8a11daf0eec32456854542a2f5df0
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/20/2021
ms.locfileid: "107768728"
---
# <a name="lock-resources-to-prevent-unexpected-changes"></a>Bloquear recursos para prevenir alterações inesperadas

Como administrador, pode bloquear uma subscrição, grupo de recursos ou recurso para evitar que outros utilizadores da sua organização apaguem ou modifiquem acidentalmente recursos críticos. O bloqueio substitui quaisquer permissões que o utilizador possa ter.

Pode definir o nível do bloqueio para **CanNotDelete** ou **ReadOnly**. No portal, as fechaduras são chamadas **de Delete** e **Read-only,** respectivamente.

* **CanNotDelete** significa que os utilizadores autorizados ainda podem ler e modificar um recurso, mas não podem eliminar o recurso.
* **ReadOnly** significa que os utilizadores autorizados podem ler um recurso, mas não podem eliminar ou atualizar o recurso. A aplicação deste bloqueio é semelhante a restringir todos os utilizadores autorizados às permissões concedidas pela função **Reader.**

## <a name="how-locks-are-applied"></a>Como os bloqueios são aplicados

Quando se aplica uma fechadura no âmbito dos pais, todos os recursos dentro desse âmbito herdam a mesma fechadura. Até os recursos que adicionas mais tarde herda a fechadura do progenitor. O bloqueio mais restritivo na herança tem precedência.

Ao contrário do controlo de acesso baseado em funções, pode utilizar a gestão de bloqueios para aplicar uma restrição a todos os utilizadores e a todas as funções. Para saber mais sobre a definição de permissões para utilizadores e funções, consulte [o controlo de acesso baseado em funções Azure (Azure RBAC)](../../role-based-access-control/role-assignments-portal.md).

Os bloqueios do Resource Manager aplicam-se apenas a operações que ocorrem no painel de gestão, o que consiste em operações enviadas para `https://management.azure.com`. Os bloqueios não restringem a forma como os recursos desempenham as suas próprias funções. As alterações dos recursos são restritas, mas as operações dos recursos não o são. Por exemplo, um bloqueio ReadOnly num servidor lógico SQL Database impede-o de eliminar ou modificar o servidor. Não o impede de criar, atualizar ou eliminar dados nas bases de dados desse servidor. As transações de dados são permitidas porque essas operações não são enviadas para `https://management.azure.com`.

## <a name="considerations-before-applying-locks"></a>Considerações antes de aplicar fechaduras

A aplicação de bloqueios pode levar a resultados inesperados porque algumas operações que não parecem modificar o recurso requerem realmente ações que são bloqueadas pelo bloqueio. O Bloqueio impedirá qualquer operação que exija um pedido de CORREIO à API gestora de recursos Azure. Alguns exemplos comuns das operações que são bloqueadas por fechaduras são:

* Um bloqueio apenas de leitura numa **conta de armazenamento** impede os utilizadores de listar as chaves da conta. A operação Azure Storage [List Keys](/rest/api/storagerp/storageaccounts/listkeys) é gerida através de um pedido DEM para proteger o acesso às chaves da conta, que fornecem acesso completo aos dados na conta de armazenamento. Quando um bloqueio apenas de leitura é configurado para uma conta de armazenamento, os utilizadores que não têm as chaves da conta devem usar credenciais AD AD Azure para aceder a dados de bolha ou fila. Um bloqueio só de leitura também impede a atribuição de funções Azure RBAC que são telescópios na conta de armazenamento ou num recipiente de dados (recipiente blob ou fila).

* Um bloqueio não pode eliminar uma **conta de armazenamento** não impede que os dados dentro dessa conta sejam eliminados ou modificados. Este tipo de bloqueio apenas protege a própria conta de armazenamento de ser eliminada, e não protege os dados blob, fila, tabela ou ficheiro dentro dessa conta de armazenamento. 

* Um bloqueio apenas de leitura numa **conta de armazenamento** não impede que os dados dentro dessa conta sejam eliminados ou modificados. Este tipo de bloqueio apenas protege a própria conta de armazenamento de ser eliminada ou modificada, e não protege os dados blob, fila, tabela ou ficheiro dentro dessa conta de armazenamento. 

* Um bloqueio apenas de leitura num recurso do Serviço de **Aplicações** impede o Visual Studio Server Explorer de exibir ficheiros para o recurso porque essa interação requer acesso de escrita.

* Um bloqueio de leitura apenas num grupo de **recursos** que contenha um plano de Serviço de **Aplicações** impede-o de [aumentar ou de sair do plano](../../app-service/manage-scale-up.md).

* Um bloqueio apenas de leitura num grupo de **recursos** que contenha uma **máquina virtual** impede todos os utilizadores de iniciar ou reiniciar a máquina virtual. Estas operações requerem um pedido de CORREIO.

* Um bloqueio não pode eliminar um grupo de **recursos** impede o Gestor de Recursos Azure de [eliminar automaticamente as implementações](../templates/deployment-history-deletions.md) na história. Se atingir 800 destacamentos na história, as suas implementações falharão.

* Um bloqueio não pode eliminar o **grupo de recursos** criado pelo **Azure Backup Service** faz com que as cópias de segurança falhem. O serviço suporta um máximo de 18 pontos de restauro. Quando bloqueado, o serviço de reserva não pode limpar pontos de restauro. Para obter mais informações, consulte [Perguntas Frequentes-Back up Azure VMs](../../backup/backup-azure-vm-backup-faq.yml).

* Um bloqueio de leitura numa **subscrição** impede o **Azure Advisor** de funcionar corretamente. O consultor não consegue armazenar os resultados das suas consultas.

## <a name="who-can-create-or-delete-locks"></a>Quem pode criar ou apagar fechaduras

Para criar ou eliminar bloqueios de gestão, tem de ter acesso `Microsoft.Authorization/*` ou `Microsoft.Authorization/locks/*` ações. Das funções incorporadas, apenas **Proprietário** e **Administrador de Acesso dos Utilizadores** têm acesso a essas ações.

## <a name="managed-applications-and-locks"></a>Aplicações e fechaduras geridas

Alguns serviços da Azure, como o Azure Databricks, utilizam [aplicações geridas](../managed-applications/overview.md) para implementar o serviço. Nesse caso, o serviço cria dois grupos de recursos. Um grupo de recursos contém uma visão geral do serviço e não está bloqueado. O outro grupo de recursos contém a infraestrutura para o serviço e está bloqueado.

Se tentar eliminar o grupo de recursos de infraestrutura, obtém-se um erro indicando que o grupo de recursos está bloqueado. Se tentar apagar o bloqueio do grupo de recursos de infraestrutura, obtém-se um erro afirmando que o bloqueio não pode ser eliminado porque é propriedade de uma aplicação do sistema.

Em vez disso, elimine o serviço, que também elimina o grupo de recursos de infraestrutura.

Para aplicações geridas, selecione o serviço que implementou.

![Selecione serviço](./media/lock-resources/select-service.png)

Note que o serviço inclui um link para um **Grupo de Recursos Geridos**. Este grupo de recursos detém a infraestrutura e está bloqueado. Não pode ser apagado diretamente.

![Mostrar grupo gerido](./media/lock-resources/show-managed-group.png)

Para eliminar tudo o que for para o serviço, incluindo o grupo de recursos de infraestrutura bloqueado, **selecione Delete** para o serviço.

![Apagar serviço](./media/lock-resources/delete-service.png)

## <a name="configure-locks"></a>Fechaduras de configuração

### <a name="portal"></a>Portal

[!INCLUDE [resource-manager-lock-resources](../../../includes/resource-manager-lock-resources.md)]

### <a name="arm-template"></a>Modelo ARM

Ao utilizar um modelo de Gestor de Recursos Azure (modelo ARM) para implementar uma fechadura, é necessário estar atento ao alcance do bloqueio e ao âmbito da implantação. Para aplicar um bloqueio no âmbito de implementação, como bloquear um grupo de recursos ou subscrição, não desabar a propriedade de âmbito. Ao bloquear um recurso dentro do âmbito de implantação, desaprova a propriedade de âmbito.

O modelo a seguir aplica um bloqueio ao grupo de recursos para o qual é implantado. Note que não existe uma propriedade de âmbito no recurso de bloqueio porque o âmbito do bloqueio corresponde ao âmbito de implementação. Este modelo é implantado ao nível do grupo de recursos.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {  
    },
    "resources": [
        {
            "type": "Microsoft.Authorization/locks",
            "apiVersion": "2016-09-01",
            "name": "rgLock",
            "properties": {
                "level": "CanNotDelete",
                "notes": "Resource Group should not be deleted."
            }
        }
    ]
}
```

Para criar um grupo de recursos e bloqueá-lo, implemente o modelo seguinte ao nível de subscrição.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2018-05-01/subscriptionDeploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "rgName": {
            "type": "string"
        },
        "rgLocation": {
            "type": "string"
        }
    },
    "variables": {},
    "resources": [
        {
            "type": "Microsoft.Resources/resourceGroups",
            "apiVersion": "2019-10-01",
            "name": "[parameters('rgName')]",
            "location": "[parameters('rgLocation')]",
            "properties": {}
        },
        {
            "type": "Microsoft.Resources/deployments",
            "apiVersion": "2020-06-01",
            "name": "lockDeployment",
            "resourceGroup": "[parameters('rgName')]",
            "dependsOn": [
                "[resourceId('Microsoft.Resources/resourceGroups/', parameters('rgName'))]"
            ],
            "properties": {
                "mode": "Incremental",
                "template": {
                    "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
                    "contentVersion": "1.0.0.0",
                    "parameters": {},
                    "variables": {},
                    "resources": [
                        {
                            "type": "Microsoft.Authorization/locks",
                            "apiVersion": "2016-09-01",
                            "name": "rgLock",
                            "properties": {
                                "level": "CanNotDelete",
                                "notes": "Resource group and its resources should not be deleted."
                            }
                        }
                    ],
                    "outputs": {}
                }
            }
        }
    ],
    "outputs": {}
}
```

Ao aplicar um bloqueio a um **recurso** dentro do grupo de recursos, adicione a propriedade de âmbito. Desaprote o âmbito do nome do recurso a bloquear.

O exemplo a seguir mostra um modelo que cria um plano de serviço de aplicações, um web site e um bloqueio no site. O âmbito da fechadura está definido para o site.

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "hostingPlanName": {
      "type": "string"
    },
    "location": {
        "type": "string",
        "defaultValue": "[resourceGroup().location]"
    }
  },
  "variables": {
    "siteName": "[concat('ExampleSite', uniqueString(resourceGroup().id))]"
  },
  "resources": [
    {
      "type": "Microsoft.Web/serverfarms",
      "apiVersion": "2020-06-01",
      "name": "[parameters('hostingPlanName')]",
      "location": "[parameters('location')]",
      "sku": {
        "tier": "Free",
        "name": "f1",
        "capacity": 0
      },
      "properties": {
        "targetWorkerCount": 1
      }
    },
    {
      "type": "Microsoft.Web/sites",
      "apiVersion": "2020-06-01",
      "name": "[variables('siteName')]",
      "location": "[parameters('location')]",
      "dependsOn": [
        "[resourceId('Microsoft.Web/serverfarms', parameters('hostingPlanName'))]"
      ],
      "properties": {
        "serverFarmId": "[parameters('hostingPlanName')]"
      }
    },
    {
      "type": "Microsoft.Authorization/locks",
      "apiVersion": "2016-09-01",
      "name": "siteLock",
      "scope": "[concat('Microsoft.Web/sites/', variables('siteName'))]",
      "dependsOn": [
        "[resourceId('Microsoft.Web/sites', variables('siteName'))]"
      ],
      "properties": {
        "level": "CanNotDelete",
        "notes": "Site should not be deleted."
      }
    }
  ]
}
```

### <a name="azure-powershell"></a>Azure PowerShell

Bloqueia os recursos implantados com o Azure PowerShell utilizando o comando [New-AzResourceLock.](/powershell/module/az.resources/new-azresourcelock)

Para bloquear um recurso, forneça o nome do recurso, o seu tipo de recurso e o seu nome de grupo de recursos.

```azurepowershell-interactive
New-AzResourceLock -LockLevel CanNotDelete -LockName LockSite -ResourceName examplesite -ResourceType Microsoft.Web/sites -ResourceGroupName exampleresourcegroup
```

Para bloquear um grupo de recursos, forneça o nome do grupo de recursos.

```azurepowershell-interactive
New-AzResourceLock -LockName LockGroup -LockLevel CanNotDelete -ResourceGroupName exampleresourcegroup
```

Para obter informações sobre um bloqueio, utilize [o Get-AzResourceLock](/powershell/module/az.resources/get-azresourcelock). Para obter todas as fechaduras da sua subscrição, utilize:

```azurepowershell-interactive
Get-AzResourceLock
```

Para obter todas as fechaduras para um recurso, use:

```azurepowershell-interactive
Get-AzResourceLock -ResourceName examplesite -ResourceType Microsoft.Web/sites -ResourceGroupName exampleresourcegroup
```

Para obter todas as fechaduras de um grupo de recursos, utilize:

```azurepowershell-interactive
Get-AzResourceLock -ResourceGroupName exampleresourcegroup
```

Para eliminar um bloqueio para um recurso, utilize:

```azurepowershell-interactive
$lockId = (Get-AzResourceLock -ResourceGroupName exampleresourcegroup -ResourceName examplesite -ResourceType Microsoft.Web/sites).LockId
Remove-AzResourceLock -LockId $lockId
```

Para eliminar uma fechadura para um grupo de recursos, utilize:

```azurepowershell-interactive
$lockId = (Get-AzResourceLock -ResourceGroupName exampleresourcegroup).LockId
Remove-AzResourceLock -LockId $lockId
```

### <a name="azure-cli"></a>CLI do Azure

Bloqueia os recursos implantados com o Azure CLI utilizando o comando [de criação de bloqueio az.](/cli/azure/lock#az_lock_create)

Para bloquear um recurso, forneça o nome do recurso, o seu tipo de recurso e o seu nome de grupo de recursos.

```azurecli
az lock create --name LockSite --lock-type CanNotDelete --resource-group exampleresourcegroup --resource-name examplesite --resource-type Microsoft.Web/sites
```

Para bloquear um grupo de recursos, forneça o nome do grupo de recursos.

```azurecli
az lock create --name LockGroup --lock-type CanNotDelete --resource-group exampleresourcegroup
```

Para obter informações sobre uma fechadura, utilize [a lista de bloqueios az](/cli/azure/lock#az_lock_list). Para obter todas as fechaduras da sua subscrição, utilize:

```azurecli
az lock list
```

Para obter todas as fechaduras para um recurso, use:

```azurecli
az lock list --resource-group exampleresourcegroup --resource-name examplesite --namespace Microsoft.Web --resource-type sites --parent ""
```

Para obter todas as fechaduras de um grupo de recursos, utilize:

```azurecli
az lock list --resource-group exampleresourcegroup
```

Para eliminar um bloqueio para um recurso, utilize:

```azurecli
lockid=$(az lock show --name LockSite --resource-group exampleresourcegroup --resource-type Microsoft.Web/sites --resource-name examplesite --output tsv --query id)
az lock delete --ids $lockid
```

Para eliminar uma fechadura para um grupo de recursos, utilize:

```azurecli
lockid=$(az lock show --name LockSite --resource-group exampleresourcegroup  --output tsv --query id)
az lock delete --ids $lockid
```

### <a name="rest-api"></a>API REST

Pode bloquear recursos implantados com a [API REST para bloqueios de gestão](/rest/api/resources/managementlocks). A API REST permite-lhe criar e eliminar fechaduras e recuperar informações sobre fechaduras existentes.

Para criar uma fechadura, corra:

```http
PUT https://management.azure.com/{scope}/providers/Microsoft.Authorization/locks/{lock-name}?api-version={api-version}
```

O âmbito pode ser uma subscrição, grupo de recursos ou recurso. O nome do cadeado é o que quiser chamar de fechadura. Para a versão api, utilize **2016-09-01**.

No pedido, inclua um objeto JSON que especifica as propriedades para o bloqueio.

```json
{
  "properties": {
  "level": "CanNotDelete",
  "notes": "Optional text notes."
  }
}
```

## <a name="next-steps"></a>Passos seguintes

* Para aprender a organizar logicamente os seus recursos, consulte [Usando tags para organizar os seus recursos.](tag-resources.md)
* Pode aplicar restrições e convenções em toda a sua subscrição com políticas personalizadas. Para obter mais informações, veja [What is Azure Policy?](../../governance/policy/overview.md) (O que é o Azure Policy?).
* Para obter documentação de orientação sobre como as empresas podem utilizar o Resource Manager para gerir subscrições de forma eficaz, consulte [Azure enterprise scaffold - prescriptive subscription governance (Andaime empresarial do Azure - governação de subscrições prescritivas)](/azure/architecture/cloud-adoption-guide/subscription-governance).
