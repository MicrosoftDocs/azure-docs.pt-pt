---
title: Monitorize uma máquina virtual Azure com monitor Azure
description: Saiba como recolher e analisar dados para uma máquina virtual Azure no Azure Monitor.
ms.service: azure-monitor
ms.topic: quickstart
author: bwren
ms.author: bwren
ms.date: 03/10/2020
ms.openlocfilehash: 7efd8baf54aeacbd2f55640240a15f2517dcd904
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/20/2021
ms.locfileid: "102046929"
---
# <a name="quickstart-monitor-an-azure-virtual-machine-with-azure-monitor"></a>Quickstart: Monitorize uma máquina virtual Azure com monitor Azure
[O Azure Monitor](../overview.md) começa a recolher dados de máquinas virtuais Azure no momento em que são criados. Neste arranque rápido, você vai fazer uma breve caminhada através dos dados que são automaticamente recolhidos para um Azure VM e como vê-lo no portal Azure. Em seguida, irá ativar [informações em VM](../vm/vminsights-overview.md) para o seu VM, que permitirá aos agentes do VM recolher e analisar dados do sistema operativo convidado, incluindo processos e suas dependências.

Este início rápido pressupõe que tem uma máquina virtual do Azure. Caso contrário, pode criar um [VM do Windows](../../virtual-machines/windows/quick-create-portal.md) ou criar um [VM Linux](../../virtual-machines/linux/quick-create-cli.md) seguindo os nossos quickstarts VM.

Para descrições mais detalhadas dos dados de monitorização recolhidos a partir dos recursos do Azure consulte [a monitorização das máquinas virtuais Azure com o Azure Monitor](./monitor-vm-azure.md).


## <a name="complete-the-monitor-an-azure-resource-quickstart"></a>Complete o Monitor de um quickstart de recurso Azure.
Complete [Monitor um recurso Azure com O Monitor Azure](../essentials/quick-monitor-azure-resource.md) para ver a página geral, registo de atividade e métricas para um VM na sua subscrição. Os VMs Azure recolhem os mesmos dados de monitorização que qualquer outro recurso Azure, mas este é apenas para o VM anfitrião. O resto deste quickstart irá focar-se na monitorização do sistema operativo dos hóspedes e das suas cargas de trabalho.


## <a name="enable-vm-insights"></a>Ativar insights VM
Enquanto as métricas e registos de atividade serão recolhidos para o VM anfitrião, você precisa de um agente e alguma configuração para recolher e analisar dados de monitorização do sistema operativo do hóspede e suas cargas de trabalho. A VM insights instala estes agentes e fornece funcionalidades poderosas adicionais para monitorizar as suas máquinas virtuais.

1. Vá ao menu da sua máquina virtual.
2. Clique em **'Insights'** a partir do azulejo na página **'Vista Geral',** ou clique em Insights a partir do menu **'Monitorização'.** 

    ![Página de descrição geral](media/quick-monitor-azure-vm/overview-insights.png)

3. Se os conhecimentos VM ainda não estavam ativados para a máquina virtual, clique em **Ativar**. 

    ![Permitir insights](media/quick-monitor-azure-vm/enable-insights.png)

4. Se a máquina virtual ainda não estiver ligada a um espaço de trabalho do Log Analytics, será solicitado que selecione um espaço de trabalho existente ou crie um novo. Selecione o padrão que é um espaço de trabalho com um nome único na mesma região que a sua máquina virtual.

    ![Selecionar área de trabalho](media/quick-monitor-azure-vm/select-workspace.png)

5. O embarque demorará alguns minutos à medida que as extensões estiverem ativadas e os agentes forem instalados na sua máquina virtual. Quando está completo, recebe-se uma mensagem de que as informações foram implementadas com sucesso. Clique em **Azure Monitor** para abrir insights em VM.

    ![Monitor aberto do Azure](media/quick-monitor-azure-vm/azure-monitor.png)

6. Verá o seu VM com quaisquer outros VMs na sua subscrição que estão a bordo. Selecione o separador **Não monitorizado** se pretender ver máquinas virtuais na sua subscrição que não estejam a bordo.

    ![Introdução](media/quick-monitor-azure-vm/get-started.png)


## <a name="configure-workspace"></a>Configurar a área de trabalho
Quando cria um novo espaço de trabalho log Analytics, tem de ser configurado para recolher registos. Esta configuração só precisa de ser executada uma vez que a configuração é enviada para quaisquer máquinas virtuais que se conectem a ela.

1. Selecione **a configuração do Espaço de Trabalho** e, em seguida, selecione o seu espaço de trabalho.

2. Selecione **configurações avançadas**

    ![Definições Avançadas do Log Analytics](../vm/media/quick-collect-azurevm/log-analytics-advanced-settings-azure-portal.png)

### <a name="data-collection-from-windows-vm"></a>Recolha de dados da VM do Windows


2. Selecione **Dados** e, em seguida, selecione **Registos de Eventos do Windows**.

3. Adicione um registo de eventos digitando o nome do registo.  Digite **sistema** e, em seguida, selecione o sinal mais **+** .

4. Na tabela, verifique as gravidades **Erro** e **Aviso**.

5. **Selecione Guardar** no topo da página para guardar a configuração.

### <a name="data-collection-from-linux-vm"></a>Recolha de dados da VM do Linux

1. Selecione **Dados** e, em seguida, selecione **Syslog**.

2. Adicione um registo de eventos digitando o nome do registo.  Escreva **Syslog** e, em seguida, selecione o sinal de mais **+** .  

3. Na tabela, desseleccione as severidades **Informação,** **Aviso** e **Debug**. 

4. **Selecione Guardar** no topo da página para guardar a configuração.

## <a name="view-data-collected"></a>Ver os dados recolhidos

7. Clique na sua máquina virtual e, em seguida, selecione o **separador Performance** que está em teia **de monitorização** **Insights.** Isto mostra um grupo selecionado de contadores de desempenho recolhidos do sistema operativo convidado do seu VM. Desloque-se para baixo para ver mais contadores e mova o rato sobre um gráfico para ver a média e percentais em momentos diferentes.

    ![A imagem mostra o painel de desempenho.](media/quick-monitor-azure-vm/performance.png)

9. Selecione **Mapa** para abrir a funcionalidade de mapas que mostra os processos em execução na máquina virtual e as suas dependências. Selecione **Propriedades** para abrir o painel de propriedades se ainda não estiver aberto.

    ![A imagem mostra o painel do mapa.](media/quick-monitor-azure-vm/map.png)

11. Expanda os processos para a sua máquina virtual. Selecione um dos processos para ver os seus detalhes e destacar as suas dependências.

    ![A screenshot mostra o painel do Mapa com processos para uma máquina virtual expandida.](media/quick-monitor-azure-vm/processes.png)

12. Selecione a sua máquina virtual novamente e, em seguida, **selecione Eventos de Registo**. 

    ![Eventos de registo](media/quick-monitor-azure-vm/log-events.png)

13. Você vê uma lista de tabelas que são armazenadas no espaço de trabalho Log Analytics para a máquina virtual. Esta lista será diferente dependendo se estiver a utilizar uma máquina virtual Windows ou Linux. Clique na tabela **Evento.** Isto inclui todos os eventos do registo de eventos do Windows. O Log Analytics abre com uma consulta simples para recuperar as entradas de registo de eventos.

    ![Log Analytics](media/quick-monitor-azure-vm/log-analytics.png)

## <a name="next-steps"></a>Passos seguintes
Neste arranque rápido, permitiu insights de VM para uma máquina virtual e configuraram o espaço de trabalho Log Analytics para recolher eventos para o sistema operativo convidado. Para saber como ver e analisar os dados, avance para o tutorial.

> [!div class="nextstepaction"]
> [Ver ou analisar dados no Log Analytics](../logs/log-analytics-tutorial.md)