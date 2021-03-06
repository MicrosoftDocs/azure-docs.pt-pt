---
title: incluir ficheiro
description: incluir ficheiro
services: azure-resource-manager
author: tfitzmac
ms.service: cost-management-billing
ms.topic: include
ms.date: 03/15/2021
ms.author: tomfitz
ms.custom: include file
ms.openlocfilehash: f02b7f0f80cfb875cc6207b542db90607b379b67
ms.sourcegitcommit: 4b0e424f5aa8a11daf0eec32456854542a2f5df0
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/20/2021
ms.locfileid: "107799943"
---
| Recurso | Limite |
| --- | --- |
| Assinaturas [associadas a um inquilino do Azure Ative Directory](../articles/active-directory/fundamentals/active-directory-how-subscriptions-associated-directory.md) | Ilimitado |
| [Coadministradores](../articles/cost-management-billing/manage/add-change-subscription-administrator.md) por subscrição |Ilimitado |
| [Grupos de recursos](../articles/azure-resource-manager/management/overview.md) por subscrição |980 |
| Tamanho do pedido de Azure Resource Manager API |4 194 304 bytes |
| Tags por subscrição<sup>1</sup> |50 |
| Cálculos exclusivos por subscrição<sup>1</sup> | 80.000 |
| [Implementações de nível de subscrição](../articles/azure-resource-manager/templates/deploy-to-subscription.md) por local | 800<sup>2</sup> |

<sup>1</sup> Pode aplicar até 50 tags diretamente a uma subscrição. No entanto, a subscrição pode conter um número ilimitado de tags que são aplicadas a grupos de recursos e recursos dentro da subscrição. O número de tags por recurso ou grupo de recursos é limitado a 50. O Gestor de Recursos devolve uma [lista de nomes e valores únicos](/rest/api/resources/tags) na subscrição apenas quando o número de tags é de 80.000 ou menos. Ainda pode encontrar um recurso por etiqueta quando o número exceder 80.000.

<sup>2</sup> Se atingir o limite de 800 implementações, elimine as implementações que já não são necessárias da história. Para eliminar as implementações de nível de subscrição, utilize [remove-AzDeployment](/powershell/module/az.resources/Remove-AzDeployment) ou [sub-eliminação de az](/cli/azure/deployment/sub#az_deployment_sub_delete).
