---
title: Ver notificações do estado de funcionamento do serviço ao utilizar o portal do Azure
description: Consulte as notificações de saúde do seu serviço no portal Azure. As notificações de saúde do serviço são publicadas pela infraestrutura Azure no registo de atividades do Azure.
ms.topic: conceptual
ms.date: 6/27/2019
ms.openlocfilehash: 9f9f3e7b10d9aa0014e4e00e7bfa72c9dc66e142
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/29/2021
ms.locfileid: "100587994"
---
# <a name="view-service-health-notifications-by-using-the-azure-portal"></a>Ver notificações do estado de funcionamento do serviço ao utilizar o portal do Azure

As notificações de saúde do serviço são publicadas pela infraestrutura Azure no registo de atividades do [Azure](../azure-monitor/essentials/platform-logs-overview.md).  As notificações contêm informações sobre os recursos sob a sua subscrição. Dado o eventual grande volume de informação armazenada no registo de atividades, existe uma interface de utilizador separada para facilitar a visualização e a configuração de alertas nas notificações de saúde do serviço. 

As notificações de saúde do serviço podem ser informativas ou accuais, dependendo da classe.

Para obter mais informações sobre as várias classes de notificações de saúde do serviço, consulte [as propriedades das notificações de saúde do Serviço.](service-health-notifications-properties.md)

## <a name="view-your-service-health-notifications-in-the-azure-portal"></a>Veja as notificações de saúde do seu serviço no portal Azure

1. No [portal Azure,](https://portal.azure.com)selecione **Monitor**.

    ![Screenshot do menu do portal Azure, com Monitor selecionado](./media/service-notifications/home-monitor.png)

    O Azure Monitor reúne todas as suas definições de monitorização e dados numa única visão consolidada. É aberto pela primeira vez para a secção **Registo de atividade**.

1. Selecione **Alertas**.

    ![Screenshot do registo de atividade do monitor, com alertas selecionados](./media/service-notifications/service-health-summary.png)

1. Selecione **+Adicionar o alerta de registo de atividades** e crie um alerta para garantir que é notificado para futuras notificações de serviço. Para obter mais informações, consulte [Criar alertas de registo de atividades nas notificações de serviço](./alerts-activity-log-service-notifications-portal.md).

## <a name="next-steps"></a>Passos seguintes

* Saiba mais sobre [os alertas de registo de atividades.](../azure-monitor/alerts/activity-log-alerts.md)
