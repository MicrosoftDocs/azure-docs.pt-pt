---
title: Configure notificações de e-mail para alertas do Centro de Segurança Azure
description: Saiba como afinar os tipos de e-mails enviados pelo Azure Security Center para alertas de segurança.
author: memildin
manager: rkarlin
ms.service: security-center
ms.topic: quickstart
ms.date: 04/07/2021
ms.author: memildin
ms.openlocfilehash: 96a389a581a9ecaddfc418824b3ebe9c780e6bd1
ms.sourcegitcommit: 6ed3928efe4734513bad388737dd6d27c4c602fd
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/07/2021
ms.locfileid: "107011592"
---
# <a name="configure-email-notifications-for-security-alerts"></a>Configure notificações de email para alertas de segurança 

Os alertas de segurança precisam de chegar às pessoas certas na sua organização. Por padrão, o Security Center envia e-mails aos proprietários de subscrições sempre que um alerta de alta gravidade é desencadeado para a sua subscrição. Esta página explica como personalizar estas notificações.

Para definir as suas preferências por e-mails de notificação, a página de **notificações** de email do Azure Security Center permite-lhe escolher:

- ***que* deve ser notificado** - Os e-mails podem ser enviados para selecionar indivíduos ou para qualquer pessoa com uma função Azure especificada para uma subscrição. 
- ***sobre o que* devem ser notificados** - Modifique os níveis de gravidade para os quais o Centro de Segurança deve enviar notificações.

Para evitar cansaço de alerta, o Centro de Segurança limita o volume de mensagens de saída. Para cada subscrição, o Centro de Segurança envia:

- um máximo de um e-mail por **6 horas** (4 e-mails por dia) para alertas **de alta gravidade**
- um máximo de um e-mail por **12 horas** (2 e-mails por dia) para alertas **de gravidade média**
- um máximo de um e-mail por **24 horas** para alertas **de baixa gravidade**

:::image type="content" source="./media/security-center-provide-security-contacts/email-notification-settings.png" alt-text="Configurar os detalhes do contacto que receberá e-mails sobre alertas de segurança." :::
 
## <a name="availability"></a>Disponibilidade

|Aspeto|Detalhes|
|----|:----|
|Estado de libertação:|Disponibilidade Geral (GA)|
|Preços:|Gratuito|
|Funções e permissões necessárias:|**Administrador de Segurança**<br>**Proprietário de Assinatura** |
|Nuvens:|![Yes](./media/icons/yes-icon.png) Nuvens comerciais<br>![Yes](./media/icons/yes-icon.png) Nacional/Soberano (Gov dos EUA, China Gov, Outro Gov)|
|||


## <a name="customize-the-security-alerts-email-notifications-via-the-portal"></a>Personalize as notificações de email alerta de segurança através do portal<a name="email"></a>
Pode enviar notificações por e-mail a indivíduos ou a todos os utilizadores com funções específicas do Azure.

1. A partir da área de **definições** de preços & do Centro de Segurança, selecione a subscrição relevante e selecione **notificações de e-mail**.

1. Defina os destinatários das suas notificações com uma ou ambas as opções:

    - Na lista de abandono, selecione entre as funções disponíveis.
    - Introduza endereços de e-mail específicos separados por vírgulas. Não há limite para o número de endereços de e-mail que pode introduzir.

1. Para aplicar as informações de contacto de segurança à sua subscrição, **selecione Guardar**.

## <a name="customize-the-alerts-email-notifications-through-the-api"></a>Personalize as notificações de email de alerta através da API
Também pode gerir as suas notificações de e-mail através da API REST fornecida. Para mais detalhes consulte a [documentação da API SecurityContacts](/rest/api/securitycenter/securitycontacts).

Este é um órgão de pedido de exemplo para o pedido PUT ao criar uma configuração de contacto de segurança:

URI: https://management.azure.com/subscriptions/ <SubscriptionId> /providers/Microsoft.Security/securityContacts/default?api-version=2020-01-01-pré-visualização

```json
{
    "properties": {
        "emails": admin@contoso.com;admin2@contoso.com,
        "notificationsByRole": {
            "state": "On",
            "roles": ["AccountAdmin", "Owner"]
        },
        "alertNotifications": {
            "state": "On",
            "minimalSeverity": "Medium"
        },
        "phone": ""
    }
}
```


## <a name="see-also"></a>Ver também
Para saber mais sobre alertas de segurança, consulte as seguintes páginas:

- [Alertas de segurança - um guia](alerts-reference.md)de referência --Saiba mais sobre os alertas de segurança que pode ver no módulo de Proteção de Ameaças do Centro de Segurança Azure
- [Gerir e responder a alertas de segurança no Azure Security Center](security-center-managing-and-responding-alerts.md)--Saiba como gerir e responder a alertas de segurança
- [Automatização do fluxo de trabalho](workflow-automation.md)--Automatizar respostas a alertas com lógica de notificação personalizada