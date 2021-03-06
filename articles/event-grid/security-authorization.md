---
title: Segurança e autenticação da Grelha de Eventos Azure
description: Descreve o Azure Event Grid e respetivos conceitos.
ms.topic: conceptual
ms.date: 02/12/2021
ms.openlocfilehash: e9bcf00e832e4deaaf9c5f81ba5af51609a1c412
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/30/2021
ms.locfileid: "104601045"
---
# <a name="authorizing-access-to-event-grid-resources"></a>Autorizar o acesso aos recursos da Grade de Eventos
O Azure Event Grid permite-lhe controlar o nível de acesso dado a diferentes utilizadores para fazer várias operações de **gestão,** tais como subscrições de eventos de lista, criar novas e gerar chaves. A Grade de Eventos utiliza o controlo de acesso baseado em funções Azure (Azure RBAC).

> [!NOTE]
> O EventGrid não suporta o Azure RBAC para publicar eventos para tópicos ou domínios de Event Grid. Utilize uma chave ou ficha de assinatura de acesso partilhado (SAS) para autenticar clientes que publiquem eventos. Para mais informações, consulte [autenticar clientes editoriais.](security-authenticate-publishing-clients.md) 

## <a name="operation-types"></a>Tipos de operação
Para uma lista de operação apoiada pela Azure Event Grid, execute o seguinte comando Azure CLI: 

```azurecli-interactive
az provider operation show --namespace Microsoft.EventGrid
```

As seguintes operações devolvem informações potencialmente secretas, que são filtradas fora das operações normais de leitura. Recomenda-se que restrinja o acesso a estas operações. 

* Microsoft.EventGrid/eventSubscriptions/getFullUrl/action
* Microsoft.EventGrid/topics/listKeys/action
* Microsoft.EventGrid/topics/regenerateKey/action


## <a name="built-in-roles"></a>Funções incorporadas
A Grade de Eventos fornece os seguintes três papéis incorporados. 

As funções de leitor de subscrição de grelha de eventos e colaborador de subscrição de grade de eventos destinam-se à gestão de subscrições de eventos. São importantes na implementação [de domínios de eventos](event-domains.md) porque dão aos utilizadores as permissões necessárias para subscreverem tópicos no domínio do evento. Estas funções estão focadas em subscrições de eventos e não dão acesso a ações como a criação de tópicos.

A função de Contribuinte de Grelha de Eventos permite-lhe criar e gerir os recursos da Grade de Eventos. 


| Função | Description |
| ---- | ----------- | 
| [Leitor de subscrição de grelha de evento](../role-based-access-control/built-in-roles.md#eventgrid-eventsubscription-reader) | Permite-lhe gerir as operações de subscrição de eventos da Event Grid. |
| [Colaborador de subscrição de grelha de evento](../role-based-access-control/built-in-roles.md#eventgrid-eventsubscription-contributor) | Permite-lhe ler subscrições de eventos Grid. |
| [Colaborador da Grelha de Eventos](../role-based-access-control/built-in-roles.md#eventgrid-contributor) | Permite criar e gerir os recursos da Grade de Eventos. |


> [!NOTE]
> Selecione links na primeira coluna para navegar para um artigo que forneça mais detalhes sobre o papel. Para obter instruções sobre como atribuir utilizadores ou grupos às funções de RBAC, consulte [este artigo](../role-based-access-control/quickstart-assign-role-user-portal.md).


## <a name="custom-roles"></a>Funções personalizadas

Se precisar de especificar permissões diferentes das funções incorporadas, pode criar funções personalizadas.

Seguem-se definições de funções de sample Event Grid que permitem aos utilizadores tomar diferentes ações. Estas funções personalizadas são diferentes das funções incorporadas porque concedem um acesso mais amplo do que apenas subscrições de eventos.

**EventGridReadOnlyRole.js:** Apenas permitir operações de leitura.

```json
{
  "Name": "Event grid read only role",
  "Id": "7C0B6B59-A278-4B62-BA19-411B70753856",
  "IsCustom": true,
  "Description": "Event grid read only role",
  "Actions": [
    "Microsoft.EventGrid/*/read"
  ],
  "NotActions": [
  ],
  "AssignableScopes": [
    "/subscriptions/<Subscription Id>"
  ]
}
```

**EventGridNoDeleteListKeysRole.jsem:** Permitir ações postadas restritas, mas não permitir a exclusão de ações.

```json
{
  "Name": "Event grid No Delete Listkeys role",
  "Id": "B9170838-5F9D-4103-A1DE-60496F7C9174",
  "IsCustom": true,
  "Description": "Event grid No Delete Listkeys role",
  "Actions": [
    "Microsoft.EventGrid/*/write",
    "Microsoft.EventGrid/eventSubscriptions/getFullUrl/action"
    "Microsoft.EventGrid/topics/listkeys/action",
    "Microsoft.EventGrid/topics/regenerateKey/action"
  ],
  "NotActions": [
    "Microsoft.EventGrid/*/delete"
  ],
  "AssignableScopes": [
    "/subscriptions/<Subscription id>"
  ]
}
```

**EventGridContributorRole.jsem**: Permite todas as ações da grelha de eventos.

```json
{
  "Name": "Event grid contributor role",
  "Id": "4BA6FB33-2955-491B-A74F-53C9126C9514",
  "IsCustom": true,
  "Description": "Event grid contributor role",
  "Actions": [
    "Microsoft.EventGrid/*/write",
    "Microsoft.EventGrid/*/delete",
    "Microsoft.EventGrid/topics/listkeys/action",
    "Microsoft.EventGrid/topics/regenerateKey/action",
    "Microsoft.EventGrid/eventSubscriptions/getFullUrl/action"
  ],
  "NotActions": [],
  "AssignableScopes": [
    "/subscriptions/<Subscription id>"
  ]
}
```

Pode criar funções personalizadas com [PowerShell,](../role-based-access-control/custom-roles-powershell.md) [Azure CLI](../role-based-access-control/custom-roles-cli.md)e [REST](../role-based-access-control/custom-roles-rest.md).



### <a name="encryption-at-rest"></a>Encriptação de dados inativos

Todos os eventos ou dados escritos em disco pelo serviço Desempenhado pelo Serviço de Grelha de Eventos são encriptados por uma chave gerida pela Microsoft, garantindo que está encriptada em repouso. Adicionalmente, o período máximo de tempo que os eventos ou dados retidos são de 24 horas de adesão à [política de relíndi da Grelha de Eventos.](delivery-and-retry.md) A Grelha de Eventos eliminará automaticamente todos os eventos ou dados após 24 horas, ou o tempo de vida do evento, o que for menor.

## <a name="permissions-for-event-subscriptions"></a>Permissões para subscrições de eventos
Se estiver a usar um manipulador de eventos que não seja um WebHook (como um centro de eventos ou armazenamento de fila), precisa de escrever acesso a esse recurso. Esta verificação de permissões impede um utilizador não autorizado de enviar eventos para o seu recurso.

Tem de ter a permissão **Microsoft.EventGrid/EventSubscriptions/Write** no recurso que é a fonte do evento. Precisa desta permissão porque está a escrever uma nova subscrição no âmbito do recurso. O recurso necessário difere com base no facto de estar a subscrever um tópico do sistema ou um tópico personalizado. Ambos os tipos são descritos nesta secção.

### <a name="system-topics-azure-service-publishers"></a>Tópicos do sistema (editores de serviços Azure)
Para os tópicos do sistema, se não for o proprietário ou colaborador do recurso de origem, necessita de autorização para escrever uma nova subscrição de eventos no âmbito da publicação de recursos do evento. O formato do recurso é: `/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/{resource-provider}/{resource-type}/{resource-name}`

Por exemplo, para subscrever um evento numa conta de armazenamento chamada **myacct,** precisa da permissão microsoft.EventGrid/EventSubscriptions/Write on: `/subscriptions/####/resourceGroups/testrg/providers/Microsoft.Storage/storageAccounts/myacct`

### <a name="custom-topics"></a>Tópicos personalizados
Para tópicos personalizados, você precisa de permissão para escrever uma nova subscrição de evento no âmbito do tópico da grelha de eventos. O formato do recurso é: `/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.EventGrid/topics/{topic-name}`

Por exemplo, para subscrever um tópico personalizado chamado **mytopic,** precisa da permissão microsoft.EventGrid/EventSubscriptions/Write em: `/subscriptions/####/resourceGroups/testrg/providers/Microsoft.EventGrid/topics/mytopic`



## <a name="next-steps"></a>Passos seguintes

* Para uma introdução à Grade de Eventos, consulte [Sobre a Grelha de Eventos](overview.md)
