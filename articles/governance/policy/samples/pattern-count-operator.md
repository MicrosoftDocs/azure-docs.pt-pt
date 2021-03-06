---
title: 'Padrão: O operador de contagem numa definição de política'
description: Este padrão de Política Azure fornece um exemplo de como usar o operador de contagem numa definição de política.
ms.date: 03/31/2021
ms.topic: sample
ms.openlocfilehash: dc2914028887ae5a91e3379e2a94ddbc57a7cef3
ms.sourcegitcommit: 99fc6ced979d780f773d73ec01bf651d18e89b93
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/31/2021
ms.locfileid: "106093457"
---
# <a name="azure-policy-pattern-the-count-operator"></a>Padrão de política Azure: o operador de contagem

O operador [da contagem](../concepts/definition-structure.md#count) avalia membros de \[ \* \] um pseudónimo.

## <a name="sample-policy-definition"></a>Definição de política de amostra

Esta definição de política [audita grupos](../concepts/effects.md#audit) de segurança de rede configurados para permitir o tráfego de tráfego de protocolo remoto de ambiente de trabalho (RDP) de entrada.

:::code language="json" source="~/policy-templates/patterns/pattern-count-operator.json":::

### <a name="explanation"></a>Explicação

Os componentes centrais do operador da **contagem** são _o campo,_ _onde,_ e a condição. Cada um é destacado no corte abaixo.

- _campo_ diz a contagem de quais pseudónimos para avaliar [membros](../concepts/definition-structure.md#aliases) de. Aqui, estamos a olhar para o _conjunto_ de **seguranças \[ \* \]** do grupo de segurança da rede.
- _onde_ utiliza a linguagem política para definir quais os membros _da matriz_ que satisfazem os critérios. Neste exemplo, um **operador lógico** agrulia três avaliações de condições diferentes de propriedades de _matriz_ de pseudónimos: _direção,_ _acesso_ e _destinoPortRange._
- A condição de contagem neste exemplo é **maior.** O Conde avalia como verdadeiro quando um ou mais membros da _matriz_ de pseudónimos correspondem à cláusula _de onde._

:::code language="json" source="~/policy-templates/patterns/pattern-count-operator.json" range="12-32" highlight="3,4,20":::

## <a name="next-steps"></a>Passos seguintes

- Reveja [outros padrões e definições incorporadas.](./index.md)
- Reveja a [estrutura de definição do Azure Policy](../concepts/definition-structure.md).
- Veja [Compreender os efeitos do Policy](../concepts/effects.md).