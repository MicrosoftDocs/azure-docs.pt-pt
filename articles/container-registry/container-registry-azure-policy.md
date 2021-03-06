---
title: Conformidade usando a Política Azure
description: Atribuir políticas incorporadas na Política Azure para auditar o cumprimento dos seus registos de contentores Azure
ms.topic: article
ms.date: 03/01/2021
ms.openlocfilehash: 62a1fd8d3c996fd3a0bac3cadf77fc7e7ace0ce3
ms.sourcegitcommit: 4b0e424f5aa8a11daf0eec32456854542a2f5df0
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/20/2021
ms.locfileid: "107784178"
---
# <a name="audit-compliance-of-azure-container-registries-using-azure-policy"></a>Conformidade de auditoria dos registos de contentores da Azure utilizando a Política Azure

[A Azure Policy](../governance/policy/overview.md) é um serviço em Azure que usa para criar, atribuir e gerir políticas. Estas políticas impõem diferentes regras e efeitos aos recursos, de forma a que esses recursos se mantenham em conformidade com as normas empresariais e os contratos de nível de serviço.

Este artigo introduz políticas incorporadas para o Registo de Contentores Azure. Utilize estas políticas para auditar novos registos existentes para o cumprimento.

Não há acusações por usar a Política Azure.

## <a name="built-in-policy-definitions"></a>Definições políticas incorporadas

As seguintes definições de política incorporadas são específicas do Registo de Contentores de Azure:

[!INCLUDE [azure-policy-reference-rp-containerreg](../../includes/policy/reference/byrp/microsoft.containerregistry.md)]

## <a name="assign-policies"></a>Políticas de atribuição

* Atribuir políticas utilizando o [portal Azure](../governance/policy/assign-policy-portal.md), [Azure CLI,](../governance/policy/assign-policy-azurecli.md)um [modelo de Gestor de Recursos,](../governance/policy/assign-policy-template.md)ou os SDKs de Política Azure.
* Âmbito de uma atribuição de políticas a um grupo de recursos, a uma subscrição ou a um [grupo de gestão Azure](../governance/management-groups/overview.md). As atribuições da política de registo de contentores aplicam-se aos registos existentes e novos de contentores no âmbito de aplicação.
* Ativar ou desativar [a aplicação da política a](../governance/policy/concepts/assignment-structure.md#enforcement-mode) qualquer momento.

> [!NOTE]
> Depois de atribuir ou atualizar uma apólice, leva algum tempo para que a atribuição seja aplicada aos recursos no âmbito definido. Consulte informações sobre [os gatilhos de avaliação de políticas](../governance/policy/how-to/get-compliance-data.md#evaluation-triggers).

## <a name="review-policy-compliance"></a>Rever cumprimento da política

Aceder a informações de conformidade geradas pelas suas atribuições de política utilizando o portal Azure, ferramentas de linha de comando Azure ou os SDKs de política Azure. Para mais informações, consulte [obter dados de conformidade dos recursos da Azure.](../governance/policy/how-to/get-compliance-data.md)

Quando um recurso não é conforme, existem muitas razões possíveis. Para determinar a razão ou para encontrar a alteração responsável, consulte [determinar o incumprimento](../governance/policy/how-to/determine-non-compliance.md).

### <a name="policy-compliance-in-the-portal"></a>Conformidade da política no portal:

1. Selecione **Todos os serviços** e procure **por Política.**
1. Selecione **Compliance**.
1. Utilize os filtros para limitar os estados de conformidade ou para procurar políticas.

    ![Conformidade de política no portal](./media/container-registry-azure-policy/azure-policy-compliance.png)
    
1. Selecione uma política para rever detalhes e eventos de conformidade agregados. Se desejar, selecione um registo específico para a conformidade com os recursos.

### <a name="policy-compliance-in-the-azure-cli"></a>Conformidade da política no Azure CLI

Também pode utilizar o CLI Azure para obter dados de conformidade. Por exemplo, utilize o comando da [lista de atribuição de políticas az](/cli/azure/policy/assignment#az_policy_assignment_list) no CLI para obter os IDs de política das políticas de registo de contentores Azure que são aplicadas:

```azurecli
az policy assignment list --query "[?contains(displayName,'Container Registries')].{name:displayName, ID:id}" --output table
```

Resultado do exemplo:

```
Name                                                                                   ID
-------------------------------------------------------------------------------------  --------------------------------------------------------------------------------------------------------------------------------
Container Registries should not allow unrestricted network access           /subscriptions/<subscriptionID>/providers/Microsoft.Authorization/policyAssignments/b4faf132dc344b84ba68a441
Container Registries should be encrypted with a Customer-Managed Key (CMK)  /subscriptions/<subscriptionID>/providers/Microsoft.Authorization/policyAssignments/cce1ed4f38a147ad994ab60a
```

Em [seguida,](/cli/azure/policy/state#az_policy_state_list) executar a lista de estado de política az para devolver o estado de conformidade formatado json para todos os recursos sob uma determinada iD de política:

```azurecli
az policy state list \
  --resource <policyID>
```

Ou executar [a lista de estados de política az](/cli/azure/policy/state#az_policy_state_list) para devolver o estado de conformidade formatado pelo JSON de um recurso de registo específico, como *o myregistry*:

```azurecli
az policy state list \
 --resource myregistry \
 --namespace Microsoft.ContainerRegistry \
 --resource-type registries \
 --resource-group myresourcegroup
```

## <a name="next-steps"></a>Passos seguintes

* Saiba mais sobre [definições](../governance/policy/concepts/definition-structure.md) e [efeitos](../governance/policy/concepts/effects.md)da Política Azure .

* Crie uma [definição de política personalizada.](../governance/policy/tutorials/create-custom-policy-definition.md)

* Saiba mais sobre [as capacidades de governação](../governance/index.yml) em Azure.
