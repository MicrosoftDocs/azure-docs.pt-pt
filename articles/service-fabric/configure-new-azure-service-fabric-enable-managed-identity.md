---
title: Configurar suporte de identidade gerido para um novo cluster de Tecidos de Serviço
description: Aqui está como permitir o suporte de identidades geridas em um novo cluster de tecido de serviço Azure
ms.topic: article
ms.date: 12/09/2019
ms.openlocfilehash: ae4fec452a2342a68843d874ba955b594014c46d
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/29/2021
ms.locfileid: "96574671"
---
# <a name="configure-managed-identity-support-for-a-new-service-fabric-cluster"></a>Configurar suporte de identidade gerido para um novo cluster de Tecidos de Serviço

Para utilizar [identidades geridas para recursos Azure](../active-directory/managed-identities-azure-resources/overview.md) nas suas aplicações de Tecido de Serviço, primeiro ative o *Serviço de Token de Identidade Gerida* no cluster. Este serviço é responsável pela autenticação de aplicações do Service Fabric utilizando as suas identidades geridas, e pela obtenção de fichas de acesso em seu nome. Uma vez que o serviço esteja ativado, pode vê-lo no Service Fabric Explorer sob a secção **Sistema** no painel esquerdo, funcionando sob o tecido de **nome:/System/ManagedIdentityTokenService** ao lado de outros serviços do sistema.

> [!NOTE]
> A versão de execução do tecido de serviço 6.5.658.9590 é necessária para ativar o **Serviço de Token de Identidade Gerida**.  

## <a name="enable-the-managed-identity-token-service"></a>Ativar o Serviço de Token de Identidade Gerida

Para ativar o Serviço de Token de identidade gerida no tempo de criação do cluster, adicione o seguinte corte ao modelo do cluster Azure Resource Manager:

```json
"fabricSettings": [
    {
        "name": "ManagedIdentityTokenService",
        "parameters": [
            {
                "name": "IsEnabled",
                "value": "true"
            }
        ]
    }
]
```

## <a name="errors"></a>Erros

Se a implementação falhar com esta mensagem, significa que o cluster não está na versão necessária do Service Fabric (o tempo mínimo suportado é de 6,5 CU2):


```json
{
    "code": "ParameterNotAllowed",
    "message": "Section 'ManagedIdentityTokenService' and Parameter 'IsEnabled' is not allowed."
}
```

## <a name="related-articles"></a>Artigos Relacionados

* Rever [suporte de identidade gerido](./concepts-managed-identity.md) em Azure Service Fabric

* [Permitir o suporte de identidade gerido num cluster de tecido de serviço Azure existente](./configure-existing-cluster-enable-managed-identity-token-service.md)

## <a name="next-steps"></a>Passos seguintes

* [Implementar uma aplicação Azure Service Fabric com uma identidade gerida atribuída ao sistema](./how-to-deploy-service-fabric-application-system-assigned-managed-identity.md)
* [Implementar uma aplicação Azure Service Fabric com uma identidade gerida atribuída pelo utilizador](./how-to-deploy-service-fabric-application-user-assigned-managed-identity.md)
* [Alavancar a identidade gerida de uma aplicação de Tecido de Serviço a partir do código de serviço](./how-to-managed-identity-service-fabric-app-code.md)
* [Conceder a uma aplicação Azure Service Fabric acesso a outros recursos da Azure](./how-to-grant-access-other-resources.md)