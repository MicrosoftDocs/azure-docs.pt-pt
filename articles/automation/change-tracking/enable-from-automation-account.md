---
title: Ativar o Rastreio e Inventário de Alteração de Automação da Azure
description: Este artigo diz como ativar o Rastreio de Alterações e Inventário a partir de uma conta de Automação.
services: automation
ms.subservice: change-inventory-management
ms.date: 10/14/2020
ms.topic: conceptual
ms.openlocfilehash: 32fb95c88d632cc2c51cd2390f0244e9c1927051
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/29/2021
ms.locfileid: "100585895"
---
# <a name="enable-change-tracking-and-inventory-from-an-automation-account"></a>Ativar o Controlo de Alterações e Inventário a partir de uma conta de Automatização

Este artigo descreve como pode utilizar a sua conta Demômessa para ativar o Tracking e o Inventário de [Alterações](overview.md) para VMs no seu ambiente. Para ativar os VMs Azure em escala, deve ativar um VM existente utilizando o Change Tracking and Inventory.

> [!NOTE]
> Ao permitir o Rastreio e Inventário de Alterações, apenas certas regiões são suportadas para ligar um espaço de trabalho Log Analytics e uma conta Automation. Para obter uma lista dos pares de mapeamento suportados, consulte [o mapeamento da Região para a conta de Automação e o espaço de trabalho log Analytics.](../how-to/region-mappings.md)

## <a name="prerequisites"></a>Pré-requisitos

* Subscrição do Azure. Se ainda não tiver um, pode [ativar os benefícios do seu assinante MSDN](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) ou inscrever-se numa [conta gratuita.](https://azure.microsoft.com/free/?WT.mc_id=A261C142F)
* [Conta de automatização](../automation-security-overview.md) para gerir máquinas.
* Uma [máquina virtual.](../../virtual-machines/windows/quick-create-portal.md)

## <a name="sign-in-to-azure"></a>Iniciar sessão no Azure

Inscreva-se no Azure at https://portal.azure.com .

## <a name="enable-change-tracking-and-inventory"></a>Ativar o Controlo de Alterações e Inventário

1. Navegue na sua conta de Automação e selecione **o rastreio de Inventário** ou **Alteração** em **Gestão de Configuração.**

2. Escolha o espaço de trabalho do Log Analytics e a conta de Automação e clique em **Ativar** o Rastreio de Alterações e Inventário. A configuração leva até 15 minutos para ser concluída.

    ![Ativar o Controlo de Alterações e Inventário](media/enable-from-automation-account/enable-feature.png)

## <a name="enable-azure-vms"></a>Ativar VMs Azure

1. A partir da sua conta Demômes, selecione **Inventário** ou **Alteração de Rastreio** sob **Gestão de Configuração.**

2. Clique **+ Adicione VMs Azure** e selecione um ou mais VMs da lista. As máquinas virtuais que não podem ser ativadas são acinzentadas e incapazes de serem selecionadas. Os VMs Azure podem existir em qualquer região independentemente da localização da sua conta Automation. 

3. Clique **em Ativar** para adicionar os VMs selecionados ao grupo de computador que guardou a procura da funcionalidade. Para obter mais informações, consulte [o âmbito de rastreio de alterações de limite e o âmbito de implantação do inventário.](manage-scope-configurations.md)

      [![Ativar VMs Azure](./media/enable-from-automation-account/enable-azure-vms.png)](./media/enable-from-automation-account/enable-azure-vms-expanded.png#lightbox)

## <a name="enable-non-azure-vms"></a>Ativar VMs não-Azure

As máquinas que não estão no Azure precisam de ser adicionadas manualmente. Recomendamos a instalação do agente Log Analytics para Windows ou Linux ligando primeiro a sua máquina aos [servidores ativados do Azure Arc](../../azure-arc/servers/overview.md), e, em seguida, utilizando a Política Azure para atribuir o agente Deploy Log Analytics às [máquinas de *Aría* Arco Linux ou *Windows* Azure.](../../governance/policy/samples/built-in-policies.md#monitoring) Se também planeia monitorizar as máquinas com O Monitor Azure para VMs, em vez disso, utilize o Enable Azure Monitor para a iniciativa [VMs.](../../governance/policy/samples/built-in-initiatives.md#monitoring)

1. A partir da sua conta Demômes automática **selecione Inventário** ou **Rastreio de Alteração** sob **Gestão de Configuração.**

2. Clique **em Adicionar máquina não-Azure**. Esta ação abre uma nova janela do navegador com [instruções para instalar e configurar o agente Log Analytics para o Windows para](../../azure-monitor/agents/log-analytics-agent.md) que a máquina possa começar a reportar operações de Rastreio e Inventário de Alterações. Se está a habilitar uma máquina que é atualmente gerida pelo Gestor de Operações, não é necessário um novo agente e a informação do espaço de trabalho é inserida no agente existente.

## <a name="enable-machines-in-the-workspace"></a>Ativar máquinas no espaço de trabalho

Máquinas ou máquinas instaladas manualmente que já reportam ao seu espaço de trabalho devem ser adicionadas à Azure Automation for Change Tracking and Inventory a ser ativada.

1. A partir da sua conta Demômes, selecione **Inventário** ou **Alteração de Rastreio** sob **Gestão de Configuração.**

2. **Selecione Gerir máquinas**. A opção **'Gerir máquinas'** pode ser acinzentada se tiver optado previamente pela opção **Ativar todas as máquinas disponíveis e futuras**

    ![Pesquisas guardadas](media/enable-from-automation-account/manage-machines.png)

3. Para ativar o Rastreio de Alterações e o Inventário de todas as máquinas disponíveis, selecione **Ative em todas as máquinas disponíveis** na página **'Gerir Máquinas'.** Esta ação desativa o controlo para adicionar máquinas individualmente e adiciona todas as máquinas que reportam ao espaço de trabalho ao grupo de computador que guardou a consulta de pesquisa. Quando selecionada, esta ação desativa a opção **'Gerir máquinas'.**

4. Para ativar a funcionalidade para todas as máquinas e máquinas futuras disponíveis, selecione **Ative em todas as máquinas disponíveis e futuras**. Esta opção elimina a configuração de pesquisa e alcance guardada do espaço de trabalho e abre a funcionalidade para todas as máquinas Azure e não-Azure que estão a reportar para o espaço de trabalho. Quando selecionada, esta ação desativa permanentemente a opção **'Gerir máquinas',** uma vez que não existe nenhuma configuração de âmbito.

    > [!NOTE]
    > Uma vez que esta opção elimina a configuração de pesquisa e alcance guardada no Log Analytics, é importante remover quaisquer bloqueios de eliminação no espaço de trabalho do Log Analytics antes de selecionar esta opção. Caso não o faça, a opção não removerá as configurações e deve removê-las manualmente.

5. Se necessário, pode adicionar de volta a configuração de âmbito, adicionando novamente a primeira procura guardada. Para obter mais informações, consulte [o âmbito de rastreio de alterações de limite e o âmbito de implantação do inventário.](manage-scope-configurations.md)

6. Para ativar a funcionalidade para uma ou mais máquinas, selecione **Ative em máquinas selecionadas** e clique em **Adicionar** ao lado de cada máquina para ativar a funcionalidade. Esta tarefa adiciona os nomes de máquinas selecionados ao grupo de computador que guardou a consulta de pesquisa para a funcionalidade.

## <a name="next-steps"></a>Passos seguintes

* Para obter detalhes sobre o trabalho com a funcionalidade, consulte [Gerir o Rastreio de Mudança](manage-change-tracking.md) e Gerir o [Inventário.](manage-inventory-vms.md)

* Para resolver problemas gerais com a funcionalidade, consulte [problemas de rastreio e inventário de alterações de resolução de problemas](../troubleshoot/change-tracking.md).
