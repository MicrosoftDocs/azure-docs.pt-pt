---
title: Visão geral da Azure Automation Update Management
description: Este artigo fornece uma visão geral da funcionalidade de Gestão de Atualização que implementa atualizações para as suas máquinas Windows e Linux.
services: automation
ms.subservice: update-management
ms.date: 04/01/2021
ms.topic: conceptual
ms.openlocfilehash: 62ae2eab33063416fdd6265b14dd8c30da55e174
ms.sourcegitcommit: d23602c57d797fb89a470288fcf94c63546b1314
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/01/2021
ms.locfileid: "106166705"
---
# <a name="update-management-overview"></a>Descrição geral da Gestão de Atualizações

Pode utilizar a Gestão de Atualização na Azure Automation para gerir as atualizações do sistema operativo para as suas máquinas virtuais Windows e Linux em Azure, em ambientes no local e em outros ambientes em nuvem. Pode avaliar rapidamente o estado das atualizações disponíveis em todas as máquinas de agente e gerir o processo de instalação das atualizações necessárias para os servidores.

Como prestador de serviços, pode ter a bordo vários inquilinos de clientes para o [Farol de Azure.](../../lighthouse/overview.md) O Azure Lighthouse permite-lhe realizar operações em escala através de vários inquilinos do Azure Ative Directory (Azure AD) de uma só vez, tornando as tarefas de gestão como a Update Management mais eficientes entre os inquilinos pelos quais é responsável.

> [!NOTE]
> Não é possível utilizar uma máquina configurada com a Update Management para executar scripts personalizados da Azure Automation. Esta máquina só pode executar o script de atualização assinado pela Microsoft.

> [!NOTE]
> Neste momento, não é suportado o modo de ativação da Gestão de Atualização diretamente a partir de um servidor ativado pelo Arc. Consulte [a Gestão de Atualização ativar a partir da sua conta Demômes](../../automation/update-management/enable-from-automation-account.md) para compreender os requisitos e como ativar o seu servidor.

Para descarregar e instalar *patches* de *Segurança* e Crítica disponíveis automaticamente no seu Azure VM, reveja o patch [de hóspedes VM automático](../../virtual-machines/automatic-vm-guest-patching.md) para VMs windows.

Antes de implementar a Gestão de Atualização e de ativar as suas máquinas para gestão, certifique-se de que compreende as informações nas seguintes secções.

## <a name="about-update-management"></a>Sobre a Gestão de Atualizações

As máquinas que são geridas pela Update Management dependem do seguinte para realizar avaliações e implementar atualizações:

* [Log Analytics agente](../../azure-monitor/agents/log-analytics-agent.md) para Windows ou Linux
* Configuração de Estado Pretendido do PowerShell (DSC) para Linux
* Automation Hybrid Runbook Worker (instalado automaticamente quando ativa a Gestão de Atualização na máquina)
* Microsoft Update ou [Serviços de Atualização do Servidor do Windows](/windows-server/administration/windows-server-update-services/get-started/windows-server-update-services-wsus) (WSUS) para máquinas Windows
* Ou um repositório de atualização privada ou pública para máquinas Linux

O diagrama a seguir ilustra como a Gestão de Atualização avalia e aplica atualizações de segurança a todos os servidores windows server e Linux conectados num espaço de trabalho:

![Fluxo de trabalho de gestão de atualização](./media/overview/update-mgmt-updateworkflow.png)

A Atualização A Gestão pode ser usada para implantar de forma nativa em máquinas em várias subscrições no mesmo inquilino, ou através de inquilinos que usam [a gestão de recursos delegados da Azure.](../../lighthouse/concepts/azure-delegated-resource-management.md)

Depois de um pacote ser lançado, leva 2 a 3 horas para que o patch apareça para as máquinas Linux para avaliação. Para as máquinas Windows, leva 12 a 15 horas para o patch aparecer para avaliação depois de ter sido lançado. Quando uma máquina completa uma verificação para a conformidade da atualização, o agente reencam a informação a granel para os registos do Azure Monitor. Numa máquina Windows, a verificação de conformidade é executada a cada 12 horas por defeito. Para uma máquina Linux, a verificação de conformidade é efetuada de hora em hora por defeito. Se o agente Log Analytics for reiniciado, iniciar-se-á uma verificação de conformidade dentro de 15 minutos.

Além do calendário de digitalização, a verificação para conformidade de atualização é iniciada dentro de 15 minutos após o reinício do agente Log Analytics, antes da instalação de atualização e após a instalação da atualização.

A Atualização A Gestão informa como a máquina está atualizada com base na fonte com que está configurada para sincronizar. Se a máquina do Windows estiver configurada para reportar aos Serviços de [Atualização do Windows Server](/windows-server/administration/windows-server-update-services/get-started/windows-server-update-services-wsus) (WSUS), dependendo da última sincronização do WSUS com o Microsoft Update, os resultados poderão diferir do que o Microsoft Update mostra. Este comportamento é o mesmo para as máquinas Linux que são configuradas para reportar a um repo local em vez de a um repo público.

> [!NOTE]
> Para informar adequadamente o serviço, a Gestão de Atualização requer que certos URLs e portas sejam ativados. Para saber mais sobre estes requisitos, consulte [a configuração da Rede](../automation-hybrid-runbook-worker.md#network-planning).

Pode implementar e instalar atualizações de software em máquinas que exijam as atualizações criando uma implementação programada. As atualizações classificadas como opcionais não estão incluídas no âmbito de implementação das máquinas Windows. Apenas as atualizações necessárias estão incluídas no âmbito de implementação.

A implementação programada define quais as máquinas-alvo que recebem as atualizações aplicáveis. Fá-lo especificando explicitamente determinadas máquinas ou selecionando um [grupo de computador](../../azure-monitor/logs/computer-groups.md) que se baseia em pesquisas de registo de um conjunto específico de máquinas (ou numa consulta [Azure](query-logs.md) que seleciona dinâmicamente VMs Azure com base em critérios especificados). Estes grupos diferem da [configuração](../../azure-monitor/insights/solution-targeting.md)de âmbito , que é usada para controlar a segmentagem das máquinas que recebem a configuração para ativar a Gestão de Atualização. Isto impede-os de executar e reportar a conformidade da atualização e instalar as atualizações necessárias aprovadas.

Ao definir uma implementação, também especifica um calendário para aprovar e definir um período de tempo durante o qual as atualizações podem ser instaladas. Este período chama-se janela de manutenção. Uma extensão de 20 minutos da janela de manutenção está reservada para reinicializações, assumindo que uma é necessária e selecionou a opção de reinicialização apropriada. Se a remendação demorar mais do que o esperado e houver menos de 20 minutos na janela de manutenção, não ocorrerá um reboot.

As atualizações são instaladas por runbooks na Automatização do Azure. Não se pode ver estes livros, e eles não requerem nenhuma configuração. Quando uma atualização é criada, cria um programa que inicia um runbook de atualização principal no tempo especificado para as máquinas incluídas. O diário principal inicia um livro de recortes de crianças em cada agente para instalar as atualizações necessárias.

Na data e hora especificadas na implementação da atualização, as máquinas-alvo executam a implantação em paralelo. Antes da instalação, é executada uma verificação para verificar se as atualizações ainda são necessárias. Para as máquinas de clientes WSUS, se as atualizações não forem aprovadas na WSUS, a implementação da atualização falha.

Não é suportado um sistema de máquinas registada para a Gestão de Atualização em mais de um espaço de trabalho log analytics (também referido como multihoming).

## <a name="clients"></a>Clientes

### <a name="supported-operating-systems"></a>Sistemas operativos suportados

A tabela que se segue lista os sistemas operativos suportados para avaliações de atualização e remendos. O patching requer um sistema Hybrid Runbook Worker, que é instalado automaticamente quando ativa a máquina virtual ou o servidor para gestão através da Gestão de Atualização. Para obter informações sobre os requisitos do sistema híbrido runbook worker, consulte [implementar um trabalhador de runbook híbrido windows](../automation-windows-hrw-install.md) e um Trabalhador de [Runbook Híbrido Linux](../automation-linux-hrw-install.md).

> [!NOTE]
> A avaliação da atualização das máquinas Linux só é suportada em certas regiões conforme listado na tabela de [mapeamentos](../how-to/region-mappings.md#supported-mappings)do espaço de trabalho da Conta Automation e do Log Analytics .

|Sistema operativo  |Notas  |
|---------|---------|
|Windows Server 2019 (Datacenter/Standard, incluindo o Núcleo do Servidor)<br><br>Windows Server 2016 (Datacenter/Standard excluindo o Núcleo do Servidor)<br><br>Windows Server 2012 R2 (Datacenter/Standard)<br><br>Windows Server 2012 | |
|Windows Server 2008 R2 (RTM e SP1 Standard)| A Atualização Gestão suporta avaliações e remendos para este sistema operativo. O [Trabalhador de Runbook Híbrido](../automation-windows-hrw-install.md) é suportado para o Windows Server 2008 R2. |
|CentOS 6, 7 e 8 (x64)      | Os agentes linux requerem acesso a um repositório de atualização. O patching baseado na classificação requer `yum` a devolução de dados de segurança que o CentOS não tem nas suas versões RTM. Para obter mais informações sobre patching baseado na classificação no CentOS, consulte [as classificações de Atualização no Linux](view-update-assessments.md#linux).          |
|Red Hat Enterprise 6, 7 e 8 (x64)     | Os agentes linux requerem acesso a um repositório de atualização.        |
|SUSE Linux Enterprise Server 12, 15 e 15.1 (x64)     | Os agentes linux requerem acesso a um repositório de atualização. Para SUSE 15.x, é necessário python 3 na máquina.      |
|Ubuntu 14.04 LTS, 16.04 LTS e 18.04 LTS (x64)      |Os agentes linux requerem acesso a um repositório de atualização.         |

> [!NOTE]
> A Atualização A Gestão não suporta automatizar com segurança a gestão de atualizações em todas as instâncias num conjunto de escala de máquina virtual Azure. [As atualizações automáticas de imagens de SO](../../virtual-machine-scale-sets/virtual-machine-scale-sets-automatic-upgrade.md) são o método recomendado para gerir atualizações de imagem do SO no seu conjunto de escala.

### <a name="unsupported-operating-systems"></a>Sistemas operativos não suportados

As seguintes listas de tabelas listam sistemas operativos não suportados pela Gestão de Atualização:

|Sistema operativo  |Notas  |
|---------|---------|
|Cliente Windows     | Os sistemas operativos dos clientes (como o Windows 7 e o Windows 10) não são suportados.<br> Para O Azure Windows Virtual Desktop (WVD), o método recomendado<br> para gerir atualizações é [o Gestor de Configuração do Microsoft Endpoint](../../virtual-desktop/configure-automatic-updates.md) para a gestão de correção de máquinas de clientes do Windows 10. |
|Windows Server 2016 Nano Server     | Não suportado.       |
|Azure Kubernetes Service Nodes | Não suportado. Utilize o processo de remendos descrito em [Aplicação de atualizações de segurança e kernel aos nós Linux no Serviço Azure Kubernetes (AKS)](../../aks/node-updates-kured.md)|

### <a name="system-requirements"></a>Requisitos de sistema

As seguintes informações descrevem os requisitos específicos do sistema operativo. Para obter orientações adicionais, consulte [o planeamento da rede.](#ports) Para compreender os requisitos para tls 1.2, consulte [a aplicação TLS 1.2 para a Azure Automation](../automation-managing-data.md#tls-12-enforcement-for-azure-automation).

#### <a name="windows"></a>Windows

Requisitos de software:

- .NET Quadro 4.6 ou posteriormente é necessário. [(Descarregue o quadro .NET](/dotnet/framework/install/guide-for-developers).
- É necessário windows PowerShell 5.1[(Baixar Windows Management Framework 5.1](https://www.microsoft.com/download/details.aspx?id=54616).)

Os agentes do Windows devem ser configurados para comunicar com um servidor WSUS, ou necessitam de acesso ao Microsoft Update. Para máquinas híbridas, recomendamos a instalação do agente Log Analytics para windows ligando primeiro a sua máquina aos [servidores ativados do Azure Arc](../../azure-arc/servers/overview.md), e depois utilizar a Azure Policy para atribuir o agente Deploy Log Analytics às [máquinas incorporadas do Windows Azure Arc.](../../governance/policy/samples/built-in-policies.md#monitoring) Em alternativa, se planeia monitorizar as máquinas com O Monitor Azure para VMs, em vez disso, utilize o Enable Azure Monitor para a iniciativa [VMs.](../../governance/policy/samples/built-in-initiatives.md#monitoring)

Pode utilizar a Gestão de Atualização com o Gestor de Configuração do Ponto Final do Microsoft. Para saber mais sobre cenários de integração, consulte [integrar a gestão de atualização com o Gestor de Configuração de Pontos finais do Windows](mecmintegration.md). O [agente Log Analytics para Windows](../../azure-monitor/agents/agent-windows.md) é necessário para servidores Windows geridos por sites no ambiente de Gestor de Configuração.

Por predefinição, os VMs do Windows que são implantados a partir do Azure Marketplace estão definidos para receber atualizações automáticas do Windows Update Service. Este comportamento não muda quando adiciona VMs do Windows ao seu espaço de trabalho. Se não gerir ativamente as atualizações utilizando a Gestão de Atualização, o comportamento padrão (para aplicar automaticamente atualizações) aplica-se.

> [!NOTE]
> Pode modificar a Política de Grupo para que as reinicializações da máquina só possam ser realizadas pelo utilizador, e não pelo sistema. As máquinas geridas podem ficar presas se a Update Management não tiver o direito de reiniciar a máquina sem interação manual do utilizador. Para obter mais informações, consulte [as definições de Política de Grupo configurada para atualizações automáticas](/windows-server/administration/windows-server-update-services/deploy/4-configure-group-policy-settings-for-automatic-updates).

#### <a name="linux"></a>Linux

Requisitos de software:

- A máquina requer acesso a um repositório de atualização, privado ou público.
- TLS 1.1 ou TLS 1.2 é necessário para interagir com a Gestão de Atualização.
- Python 2.x instalado.

> [!NOTE]
> A avaliação atualizada das máquinas Linux só é suportada em determinadas regiões. Consulte a tabela de [mapeamentos](../how-to/region-mappings.md#supported-mappings)do espaço de trabalho Da Automatização e do Log Analytics .

Para máquinas híbridas, recomendamos a instalação do agente Log Analytics para o Linux, ligando primeiro a sua máquina aos [servidores ativados do Azure Arc](../../azure-arc/servers/overview.md), e depois utilizar a Política Azure para atribuir o agente Deploy Log Analytics às [máquinas do Arco Linux Azure.](../../governance/policy/samples/built-in-policies.md#monitoring) Em alternativa, se planeia monitorizar as máquinas com O Monitor Azure para VMs, em vez disso, utilize o Enable Azure Monitor para a iniciativa [VMs.](../../governance/policy/samples/built-in-initiatives.md#monitoring)

Os VMs criados a partir das imagens on-demand Red Hat Enterprise Linux (RHEL) que estão disponíveis no Azure Marketplace estão registados para aceder à [Infraestrutura de Atualização de Chapéus Vermelhos (RHUI)](../../virtual-machines/workloads/redhat/redhat-rhui.md) que está implantada em Azure. Qualquer outra distribuição linux deve ser atualizada a partir do repositório de ficheiros online da distribuição utilizando métodos suportados pela distribuição.

## <a name="permissions"></a>Permissões

Para criar e gerir as implementações de atualizações, precisa de permissões específicas. Para saber mais sobre estas permissões, consulte [o acesso baseado em funções - Gestão de Atualização](../automation-role-based-access-control.md#update-management-permissions).

## <a name="update-management-components"></a>Componentes de Gestão de Atualização

A Atualização Gestão utiliza os recursos descritos nesta secção. Estes recursos são automaticamente adicionados à sua conta de Automação quando ativar a Gestão de Atualização.

### <a name="hybrid-runbook-worker-groups"></a>Grupos híbridos de trabalhadores de runbook

Depois de ativar a Gestão de Atualizações, qualquer máquina do Windows que esteja diretamente ligada ao seu espaço de trabalho Log Analytics é automaticamente configurada como um trabalhador de runbook híbrido do sistema para suportar os runbooks que suportam a Gestão de Atualização.

Cada máquina Windows gerida pela Update Management está listada no painel de trabalhadores híbridos como um grupo de trabalhadores híbridos do Sistema para a conta Automation. Os grupos usam a `Hostname FQDN_GUID` convenção de nomeação. Não podes visar estes grupos com livros na tua conta. Se tentar, a tentativa falha. Estes grupos destinam-se a suportar apenas a Gestão de Atualização. Para saber mais sobre a visualização da lista de máquinas windows configuradas como Um Trabalhador de Runbook Híbrido, consulte [os Trabalhadores de Runbook Híbridos.](../automation-hybrid-runbook-worker.md#view-system-hybrid-runbook-workers)

Pode adicionar a máquina Do Windows a um grupo de trabalhadores híbridos híbridos na sua conta Automation para suportar os runbooks da Automação se utilizar a mesma conta para a Gestão de Atualização e a filiação do grupo Híbrido Runbook Worker. Esta funcionalidade foi adicionada na versão 7.2.12024.0 do Híbrido Runbook Worker.

### <a name="management-packs"></a>Pacotes de gestão

Se o seu grupo de gestão de Gestor de Operações estiver [ligado a um espaço de trabalho Log Analytics,](../../azure-monitor/agents/om-agents.md)os seguintes pacotes de gestão são instalados no Gestor de Operações. Estes pacotes de gestão também são instalados para a Gestão de Atualizações em máquinas Windows conectadas diretamente. Não precisa de configurar nem de gerir estes pacotes de gestão.

* Pacote de Informações de Avaliação de Atualização do Microsoft System Center Advisor (Microsoft.IntelligencePacks.UpdateAssessment)
* Microsoft.IntelligencePack.UpdateAssessment.Configuration (Microsoft.IntelligencePack.UpdateAssessment.Configuration)
* Pacote de Gestão de Implementação de Atualização

> [!NOTE]
> Se tiver um grupo de gestão 1807 ou 2019 ligado a um espaço de trabalho log Analytics com agentes configurados no grupo de gestão para recolher dados de registo, precisa de anular o parâmetro `IsAutoRegistrationEnabled` e defini-lo para True na regra **Microsoft.IntelligencePacks.AzureAutomation.HybridAgent.Init.**

Para obter mais informações sobre atualizações de pacotes de gestão, consulte [os registos do Connect Operations Manager para o Azure Monitor](../../azure-monitor/agents/om-agents.md).

> [!NOTE]
> Para a Gestão de Atualizações para gerir totalmente as máquinas com o agente Log Analytics, tem de atualizar o agente Log Analytics para windows ou o agente Log Analytics para o Linux. Para saber como atualizar o agente, consulte [como atualizar um agente do Gestor de Operações.](/system-center/scom/deploy-upgrade-agents) Em ambientes que utilizam o Gestor de Operações, deve estar a executar o Gestor de Operações do Centro de Sistema 2012 R2 UR 14 ou mais tarde.

## <a name="data-collection"></a>Recolha de dados

### <a name="supported-sources"></a>Fontes apoiadas

A tabela a seguir descreve as fontes ligadas que a Atualização de Gestão suporta:

| Origem ligada | Suportado | Description |
| --- | --- | --- |
| Agentes do Windows |Yes |A Atualização Management recolhe informações sobre atualizações do sistema a partir de agentes do Windows e inicia a instalação das atualizações necessárias. |
| Agentes do Linux |Yes |Update Management recolhe informações sobre atualizações do sistema de agentes linux e, em seguida, inicia a instalação de atualizações necessárias em distribuições suportadas. |
| Grupo de gestão do Operations Manager |Yes |A Update Management recolhe informações sobre atualizações do sistema de agentes de um grupo de gestão conectado.<br/><br/>Não é necessária uma ligação direta do agente do Gestor de Operações aos registos do Monitor Azure. Os dados são reencaminhados do grupo de gestão para o espaço de trabalho Log Analytics. |

### <a name="collection-frequency"></a>Frequência da recolha

A Atualização Gestão verifica máquinas geridas para dados utilizando as seguintes regras. Pode demorar entre 30 minutos e 6 horas para o painel de instrumentos apresentar dados atualizados de máquinas geridas.

* Cada máquina Do Windows - A Gestão de Atualização faz uma verificação duas vezes por dia para cada máquina.

* Cada máquina Linux - Update Management faz uma varredura a cada hora.

A utilização média de dados por registos do Azure Monitor para uma máquina que utiliza a Atualização é de aproximadamente 25 MB por mês. Este valor é apenas uma aproximação e está sujeito a alterações, dependendo do seu ambiente. Recomendamos que monitorize o seu ambiente para acompanhar a sua utilização exata. Para obter mais informações sobre a análise da utilização dos dados do Azure Monitor Logs, consulte [Gerir a utilização e o custo.](../../azure-monitor/logs/manage-cost-storage.md)

## <a name="network-planning"></a><a name="ports"></a>Planeamento da rede

Consulte [a configuração da rede de automação Azure](../automation-network-configuration.md#hybrid-runbook-worker-and-state-configuration) para obter informações detalhadas sobre as portas, URLs e outros detalhes de rede necessários para a Gestão de Atualização.

Para as máquinas Windows, também deve permitir o tráfego para quaisquer pontos finais exigidos pelo Windows Update. Pode encontrar uma lista atualizada de pontos finais necessários em [Questões relacionadas com HTTP/Proxy](/windows/deployment/update/windows-update-troubleshooting#issues-related-to-httpproxy). Se tiver um servidor local [de Atualização do Windows,](/windows-server/administration/windows-server-update-services/plan/plan-your-wsus-deployment)também deve permitir o tráfego para o servidor especificado na sua [tecla WSUS](/windows/deployment/update/waas-wu-settings#configuring-automatic-updates-by-editing-the-registry).

Para máquinas Red Hat Linux, consulte [os IPs para os servidores de entrega de conteúdos RHUI](../../virtual-machines/workloads/redhat/redhat-rhui.md#the-ips-for-the-rhui-content-delivery-servers) para obter pontos finais necessários. Para outras distribuições Linux, consulte a documentação do seu fornecedor.

Para obter mais informações sobre as portas necessárias para o Trabalhador de Runbook Híbrido, consulte [os endereços de Gestão de Atualização para Trabalhador de Runbook Híbrido](../automation-hybrid-runbook-worker.md#update-management-addresses-for-hybrid-runbook-worker).

Se as suas políticas de segurança informática não permitirem que as máquinas da rede se conectem à internet, pode configurar um [gateway Log Analytics](../../azure-monitor/agents/gateway.md) e, em seguida, configurar a máquina para ligar através do gateway para Azure Automation e Azure Monitor.

## <a name="update-classifications"></a>Classificações de atualizações

A tabela a seguir define as classificações que a Atualização de Gestão suporta para atualizações do Windows.

|Classificação  |Descrição  |
|---------|---------|
|Atualizações críticas     | Uma atualização para um problema específico que aborda um bug crítico e não relacionado com a segurança.        |
|Atualizações de segurança     | Uma atualização para um problema específico do produto, relacionado com a segurança.        |
|Update rollups     | Um conjunto cumulativo de hotfixes que são embalados em conjunto para uma fácil implementação.        |
|Pacotes de funcionalidades     | Novas funcionalidades do produto que são distribuídas fora de uma versão do produto.        |
|Service packs     | Um conjunto cumulativo de hotfixes que são aplicados a uma aplicação.        |
|Atualizações de definições     | Uma atualização para ficheiros de vírus ou outra definição.        |
|Ferramentas     | Uma utilidade ou funcionalidade que ajuda a completar uma ou mais tarefas.        |
|Atualizações     | Uma atualização para uma aplicação ou ficheiro que está atualmente instalado.        |

A tabela seguinte define as classificações suportadas para atualizações do Linux.

|Classificação  |Descrição  |
|---------|---------|
|Atualizações críticas e de segurança     | Atualizações para um problema específico ou para um problema específico do produto, relacionado com a segurança.         |
|Outras atualizações     | Todas as outras atualizações que não são de natureza crítica ou que não são atualizações de segurança.        |

>[!NOTE]
>A classificação de atualização para máquinas Linux só está disponível quando usada em regiões de nuvem pública suportadas Azure. Não existe classificação das atualizações do Linux quando se utiliza a Atualização de Gestão nas seguintes regiões de nuvem nacionais:
>
>* Azure US Government
>* 21Vianet na China
>
> Em vez de serem classificadas, as atualizações são reportadas na categoria **Outras atualizações.**
>
> A Atualização Management utiliza dados publicados pelas distribuições suportadas, especificamente os seus ficheiros [OVAL](https://oval.mitre.org/) (Open Vulnerability and Assessment Language). Como o acesso à Internet é restrito a partir destas nuvens nacionais, a Gestão de Atualização não pode aceder aos ficheiros.

Para o Linux, a Update Management pode distinguir entre atualizações críticas e atualizações de segurança na nuvem sob classificação **Segurança** e **Outros**, enquanto exibe dados de avaliação devido ao enriquecimento de dados na nuvem. Para remendar, a Update Management baseia-se nos dados de classificação disponíveis na máquina. Ao contrário de outras distribuições, o CentOS não dispõe desta informação disponível na versão RTM. Se tiver máquinas CentOS configuradas para devolver dados de segurança para o seguinte comando, a Gestão de Atualização pode corrigir com base nas classificações.

```bash
sudo yum -q --security check-update
```

Não existe atualmente um método suportado para permitir a disponibilidade de dados de classificação nativa no CentOS. Neste momento, é prestado suporte limitado aos clientes que possam ter ativado esta funcionalidade por conta própria.

Para classificar as atualizações na versão 6 da Red Hat Enterprise, é necessário instalar o plugin de segurança yum. No Red Hat Enterprise Linux 7, o plugin já faz parte do próprio yum e não há necessidade de instalar nada. Para mais informações, consulte o seguinte artigo de [conhecimento](https://access.redhat.com/solutions/10021)da Red Hat .

Quando agenda uma atualização para ser executada numa máquina Linux, que por exemplo está configurada para instalar apenas atualizações correspondentes à classificação **de Segurança,** as atualizações instaladas podem ser diferentes das, ou são um subconjunto das atualizações correspondentes a esta classificação. Quando é realizada uma avaliação das atualizações do SO pendentes para a sua máquina Linux, os ficheiros [Open Vulnerability and Assessment Language](https://oval.mitre.org/) (OVAL) fornecidos pelo fornecedor de distro Linux são utilizados pela Update Management para classificação.

A categorização é feita para atualizações do Linux como **Segurança** ou **Outras** com base nos ficheiros OVA, que inclui atualizações que abordam problemas de segurança ou vulnerabilidades. Mas quando o calendário de atualização é executado, executa na máquina Linux usando o gestor de pacotes apropriado como YUM, APT ou ZYPPER para instalá-los. O gestor de pacotes para o distro Linux pode ter um mecanismo diferente para classificar as atualizações, onde os resultados podem diferir dos obtidos a partir dos ficheiros OVAL pela Update Management. Para verificar manualmente a máquina e perceber quais as atualizações que são relevantes para a segurança do seu gestor de pacotes, consulte a [implementação da atualização do Troubleshoot Linux](../troubleshoot/update-management.md#updates-linux-installed-different).

## <a name="integrate-update-management-with-configuration-manager"></a>Integrar gestão de atualização com gestor de configuração

Os clientes que investiram no Microsoft Endpoint Configuration Manager para gerir PCs, servidores e dispositivos móveis também dependem da força e maturidade do Gestor de Configuração para ajudar a gerir as atualizações de software. Para aprender a integrar a Gestão de Atualização com o Gestor de Configuração, consulte [integrar a gestão de atualização com o Gestor de Configuração de Pontos Finais do Windows](mecmintegration.md).

## <a name="third-party-updates-on-windows"></a>Atualizações de terceiros no Windows

A Atualização Conta com o repositório de atualização configurado localmente para atualizar sistemas Windows suportados, seja o WSUS ou o Windows Update. Ferramentas como [System Center Updates Publisher](/configmgr/sum/tools/updates-publisher) permitem-lhe importar e publicar atualizações personalizadas com a WSUS. Este cenário permite que a Gestão de Atualização atualize máquinas que utilizam o Gestor de Configuração como repositório de atualização com software de terceiros. Para aprender a configurar o Editor de Atualizações, consulte [o Editor de Atualizações de Instalação](/configmgr/sum/tools/install-updates-publisher).

## <a name="enable-update-management"></a>Ativar a Gestão de Atualizações

Aqui estão as formas de permitir a gestão de atualização e selecionar máquinas para ser gerido:

- Utilizando um [modelo de Gestor de Recursos](enable-from-template.md) Azure para implementar a Gestão de Atualização para uma nova conta de Automação ou existente e espaço de trabalho Azure Monitor Log Analytics na sua subscrição. Não configura o âmbito das máquinas que devem ser geridas, isto é executado como um passo separado após a utilização do gabarito.

- Da sua [conta de Automação](enable-from-automation-account.md) para uma ou mais máquinas Azure e não-Azure, incluindo servidores ativados pelo Arc.

- Utilizando o método do [livro](enable-from-runbook.md) de **bordo Enable-AutomationSolution.**

- Para um [Azure VM selecionado](enable-from-vm.md) a partir da página **de máquinas Virtuais** no portal Azure. Este cenário está disponível para Linux e Windows VMs.

- Para [vários VMs Azure](enable-from-portal.md) selecionando-os a partir da página **de máquinas Virtuais** no portal Azure.

> [!NOTE]
> A Gestão de Atualização requer a ligação de um espaço de trabalho log Analytics à sua conta de Automação. Para obter uma lista definitiva de regiões apoiadas, consulte [os mapeamentos do Espaço de Trabalho Azure.](../how-to/region-mappings.md) Os mapeamentos da região não afetam a capacidade de gerir VMs numa região separada da sua conta de Automação.

## <a name="next-steps"></a>Passos seguintes

* Para obter detalhes sobre o trabalho com a Gestão de Atualização, consulte [Gerir as atualizações para os seus VMs](manage-updates-for-vm.md).

* Reveja perguntas comumente sobre a Gestão de Atualização na [Azure Automation frequentemente perguntas](../automation-faq.md).