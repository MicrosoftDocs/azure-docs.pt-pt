---
title: Servidor SQL em Máquinas Virtuais do Windows em Azure FAQ | Microsoft Docs
description: Este artigo fornece respostas a perguntas frequentes sobre a execução do SQL Server em VMs Azure.
services: virtual-machines-windows
documentationcenter: ''
author: MashaMSFT
editor: ''
tags: azure-service-management
ms.assetid: 2fa5ee6b-51a6-4237-805f-518e6c57d11b
ms.service: virtual-machines-sql
ms.topic: troubleshooting
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 08/05/2019
ms.author: mathoma
ms.openlocfilehash: 02bb1f539369cf72a5d5b6503a3584069589b19e
ms.sourcegitcommit: 6f1aa680588f5db41ed7fc78c934452d468ddb84
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/19/2021
ms.locfileid: "107727353"
---
# <a name="frequently-asked-questions-for-sql-server-on-azure-vms"></a>Perguntas frequentes para SQL Server em VMs Azure
[!INCLUDE[appliesto-sqlvm](../../includes/appliesto-sqlvm.md)]

> [!div class="op_single_selector"]
> * [Windows](frequently-asked-questions-faq.md)
> * [Linux](../linux/frequently-asked-questions-faq.md)

Este artigo fornece respostas a algumas das questões mais comuns sobre a execução do [SQL Server em Máquinas Virtuais Windows Azure (VMs)](https://azure.microsoft.com/services/virtual-machines/sql-server/).

[!INCLUDE [support-disclaimer](../../../../includes/support-disclaimer.md)]

## <a name="images"></a><a id="images"></a> Imagens

1. **Que imagens de galeria de máquinas virtuais SQL Server estão disponíveis?** 

   O Azure mantém imagens de máquinas virtuais para todos os grandes lançamentos suportados do SQL Server em todas as edições tanto para Windows como Linux. Para mais informações, consulte a lista completa de [imagens do Windows VM](sql-server-on-azure-vm-iaas-what-is-overview.md#payasyougo) e [imagens Linux VM](../linux/sql-server-on-linux-vm-what-is-iaas-overview.md#create).

1. **As imagens existentes na galeria de máquinas virtuais SQL Server estão atualizadas?**

   De dois em dois meses, as imagens do SQL Server na galeria de máquinas virtuais são atualizadas com as mais recentes atualizações do Windows e Do Linux. Para as imagens do Windows, isto inclui quaisquer atualizações que sejam marcadas como importantes no Windows Update, incluindo importantes atualizações de segurança do SQL Server e pacotes de serviços. Para imagens Linux, isto inclui as mais recentes atualizações do sistema. As atualizações cumulativas do SQL Server são tratadas de forma diferente para o Linux e o Windows. Para o Linux, as atualizações cumulativas do SQL Server também estão incluídas na atualização. Mas, neste momento, os VMs do Windows não são atualizados com atualizações cumulativas do SQL Server ou do Windows Server.

1. **As imagens da máquina virtual SQL Server podem ser removidas da galeria?**

   Sim. O Azure mantém apenas uma imagem por grande versão e edição. Por exemplo, quando um novo pacote de serviços SQL Server é lançado, a Azure adiciona uma nova imagem à galeria para esse pacote de serviços. A imagem do SQL Server para o pacote de serviços anterior é imediatamente removida do portal Azure. No entanto, ainda está disponível para o fornecimento da PowerShell para os próximos três meses. Após três meses, a imagem do pacote de serviço anterior já não está disponível. Esta política de remoção também se aplicaria se uma versão SQL Server não for suportada quando chegar ao fim do seu ciclo de vida.


1. **É possível implantar uma imagem mais antiga do SQL Server que não seja visível no portal Azure?**

   Sim, usando o PowerShell. Para obter mais informações sobre a implementação de VMs do Servidor SQL utilizando o PowerShell, consulte [Como providenciar máquinas virtuais SQL Server com Azure PowerShell](create-sql-vm-powershell.md).
   
1. **É possível criar uma imagem generalizada do Azure Marketplace SQL Server do meu SQL Server VM e usá-la para implantar VMs?**

   Sim, mas deve então [registar cada SQL Server VM com a extensão SQL IaaS Agent](sql-agent-extension-manually-register-single-vm.md) para gerir o seu SQL Server VM no portal, bem como utilizar funcionalidades como patching automatizado e cópias de segurança automáticas. Ao registar-se com a extensão, também terá de especificar o tipo de licença para cada SQL Server VM.

1. **Como generalizo o SQL Server no Azure VM e uso-o para implantar novos VMs?**

   Pode implementar um VM do Servidor do Windows (sem o SQL Server instalado nele) e utilizar o processo [sysprep SQL](/sql/database-engine/install-windows/install-sql-server-using-sysprep) para generalizar o SQL Server em Azure VM (Windows) com os meios de instalação do SQL Server. Os clientes que tiverem o [Software Assurance](https://www.microsoft.com/licensing/licensing-programs/software-assurance-default?rtc=1&activetab=software-assurance-default-pivot%3aprimaryr3) poderão obter o suporte de dados de instalação no [Centro de Licenciamento em Volume](https://www.microsoft.com/Licensing/servicecenter/default.aspx). Os clientes que não têm Software Assurance podem utilizar os meios de configuração a partir de uma imagem VM do Azure Marketplace SQL Server que tem a edição desejada.

   Em alternativa, utilize uma das imagens do SQL Server do Azure Marketplace para generalizar o SQL Server no Azure VM. Note que deve eliminar a seguinte chave de registo na imagem de origem antes de criar a sua própria imagem. Se não o fizer, pode resultar no inchaço da pasta de instalação do SQL Server e/ou extensão SQL IaaS em estado de falha.

   Percurso chave do registo:  
   `Computer\HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Setup\SysPrepExternal\Specialize`

   > [!NOTE]
   > O SQL Server em VMs Azure, incluindo os implantados a partir de imagens generalizadas personalizadas, deve ser [registado com a extensão do Agente IAAS SQL](./sql-agent-extension-manually-register-single-vm.md?tabs=azure-cli%252cbash) para satisfazer os requisitos de conformidade e utilizar funcionalidades opcionais, tais como patching automatizado e cópias de segurança automáticas. A extensão também permite [especificar o tipo de licença](./licensing-model-azure-hybrid-benefit-ahb-change.md?tabs=azure-portal) para cada SQL Server VM.

1. **Posso usar o meu próprio VHD para implantar um SQL Server VM?**

   Sim, mas deve então [registar cada SQL Server VM com a extensão SQL IaaS Agent](sql-agent-extension-manually-register-single-vm.md) para gerir o seu SQL Server VM no portal, bem como utilizar funcionalidades como patching automatizado e cópias de segurança automáticas.

1. **É possível configurar configurações não mostradas na galeria de máquinas virtuais (por exemplo, Windows 2008 R2 + SQL Server 2012)?**

   N.º Para imagens de galeria de máquinas virtuais que incluam SQL Server, deve selecionar uma das imagens fornecidas através do portal Azure ou via [PowerShell](create-sql-vm-powershell.md). No entanto, tem a capacidade de implantar um VM do Windows e autoinstalar o SQL Server no mesmo. Em seguida, deve [registar o seu SQL Server VM com a extensão SQL IaaS Agent](sql-agent-extension-manually-register-single-vm.md) para gerir o seu SQL Server VM no portal Azure, bem como utilizar funcionalidades como patching automatizado e cópias de segurança automáticas. 


## <a name="creation"></a>Criação

1. **Como posso criar uma máquina virtual Azure com SQL Server?**

   O método mais fácil é criar uma máquina virtual que inclua o SQL Server. Para um tutorial sobre a inscrição no Azure e a criação de um SQL Server VM a partir do portal, consulte [a Provisão de uma máquina virtual SQL Server no portal Azure](create-sql-vm-portal.md). Pode selecionar uma imagem de máquina virtual que utilize o licenciamento do SQL Server pay-per-second ou pode usar uma imagem que lhe permite trazer a sua própria licença SQL Server. Também tem a opção de instalar manualmente o SQL Server num VM com uma edição livremente licenciada (Developer ou Express) ou reutilizando uma licença no local. Certifique-se de [registar o seu SQL Server VM com a extensão SQL IaaS Agent](sql-agent-extension-manually-register-single-vm.md) para gerir o seu SQL Server VM no portal, bem como utilizar funcionalidades como patching automatizado e cópias de segurança automáticas. Se trouxer a sua própria licença, deve ter [Mobilidade de Licença através de Garantia de Software na Azure.](https://azure.microsoft.com/pricing/license-mobility/) Para obter mais informações, consulte [Pricing guidance for SQL Server Azure VMs (Documentação de orientação sobre preços de VMs do Azure do SQL Server)](pricing-guidance.md).

1. **Como posso migrar a minha base de dados do SQL Server para a nuvem?**

   Primeiro crie uma máquina virtual Azure com uma instância SQL Server. Em seguida, migrar as suas bases de dados no local para esse caso. Para estratégias de migração de dados, consulte [a migração de uma base de dados SQL Server para o SQL Server num VM Azure](migrate-to-vm-from-sql-server.md).

## <a name="licensing"></a>Licenciamento

1. **Como posso instalar a minha cópia licenciada do SQL Server numa VM do Azure?**

   Pode fazê-lo de três formas. Se você é um cliente do Enterprise Agreement (EA), você pode providenciar uma das [imagens de máquina virtual que suporta licenças](sql-server-on-azure-vm-iaas-what-is-overview.md#BYOL), que também é conhecida como trazer a sua própria licença (BYOL). Se tiver [Garantia de Software,](https://www.microsoft.com/en-us/licensing/licensing-programs/software-assurance-default)pode ativar o [Benefício Híbrido Azure](licensing-model-azure-hybrid-benefit-ahb-change.md) numa imagem existente de pay-as-you-go (PAYG). Em alternativa, poderá copiar o suporte de dados da instalação do SQL Server para uma VM do Windows Server e, em seguida, instalar o SQL Server na VM. Certifique-se de registar o seu SQL Server VM com a [extensão](sql-agent-extension-manually-register-single-vm.md) para funcionalidades como gestão do portal, cópia de segurança automatizada e patching automatizado. 


1. **Um cliente precisa de Licenças de Acesso ao Cliente (CALs) do SQL Server para se ligar a uma imagem pay-as-you-go do SQL Server que está a ser executada nas Máquinas Virtuais Azure?**

   N.º Os clientes precisam de CALs quando usam a sua própria licença e movem o seu servidor SQL Server SA / CAL VM para Azure VMs. 

1. **Posso alterar uma VM para utilizar a minha própria licença do SQL Server caso tenha sido criada com uma das imagens da galeria pay as you go?**

   Sim. Pode facilmente mudar uma imagem de galeria pay-as-you-go (PAYG) para trazer a sua própria licença (BYOL) permitindo o [Benefício Híbrido Azure](https://azure.microsoft.com/pricing/hybrid-benefit/faq/).  Para obter mais informações, consulte [como alterar o modelo de licenciamento para um SQL Server VM](licensing-model-azure-hybrid-benefit-ahb-change.md). Atualmente, esta instalação só está disponível para clientes públicos e do Governo Azure.


1. **A mudança do modelo de licenciamento requer algum período de inatividade do SQL Server?**

   Não. [A alteração do modelo de licenciamento](licensing-model-azure-hybrid-benefit-ahb-change.md) não requer qualquer tempo de inatividade para o SQL Server, uma vez que a alteração é eficaz imediatamente e não requer um reinício do VM. No entanto, para registar o seu SQL Server VM com a extensão SQL IaaS Agent, a [extensão SQL IaaS](sql-server-iaas-agent-extension-automate-management.md) é um pré-requisito e instalar a extensão SQL IaaS em _modo completo_ reinicia o serviço SQL Server. Como tal, se a extensão SQL IaaS precisar de ser instalada, instale-a em modo _leve_ para uma funcionalidade limitada ou instale-a em _modo completo_ durante uma janela de manutenção. A extensão SQL IaaS instalada em modo _leve_ pode ser atualizada para o modo _completo_ a qualquer momento, mas requer um reinício do serviço SQL Server. 
   
1. **É possível trocar os modelos de licenciamento num SQL Server VM implantado com um modelo clássico?**

   N.º Mudar modelos de licenciamento não é suportado num VM clássico. Pode migrar o seu VM para o modelo Azure Resource Manager e registar-se com a extensão sql IaaS Agent. Uma vez registado o VM com a extensão sql IaaS Agent, as alterações do modelo de licenciamento estarão disponíveis no VM.

1. **Posso utilizar o portal do Azure para gerir várias instâncias na mesma VM?**

   Não. A gestão do portal é uma funcionalidade fornecida pela extensão do Agente IAAS SQL, que se baseia na extensão do Agente IAAS do SQL Server. Como tal, as mesmas limitações aplicam-se à extensão quanto à extensão. O portal pode gerir apenas uma instância predefinida ou uma instância nomeada, desde que tenha sido configurado corretamente. Para obter mais informações sobre estas limitações, consulte [a extensão do agente IAAS do SQL Server](sql-server-iaas-agent-extension-automate-management.md). 

1. **As subscrições CSP podem ativar o Benefício Híbrido do Azure?**

   Sim, o Benefício Híbrido do Azure está disponível para as subscrições CSP. Os clientes da CSP devem primeiro implementar uma imagem pay-as-you-go e, em seguida, [alterar o modelo](licensing-model-azure-hybrid-benefit-ahb-change.md) de licenciamento para trazer a sua própria licença.
   
 
1. **Tenho de pagar para licenciar o SQL Server numa VM do Azure se só estiver a ser utilizado para reserva/ativação pós-falha?**

   Para ter uma licença passiva gratuita para um grupo de disponibilidade secundária de espera ou instância agrupada por failover, deve cumprir todos os seguintes critérios descritos nos [Termos de Licenciamento](https://www.microsoft.com/licensing/product-licensing/products)do Produto :

   1. Tem [mobilidade de licença](https://www.microsoft.com/licensing/licensing-programs/software-assurance-license-mobility?activetab=software-assurance-license-mobility-pivot:primaryr2) através [da Software Assurance.](https://www.microsoft.com/licensing/licensing-programs/software-assurance-default?activetab=software-assurance-default-pivot%3aprimaryr3) 
   1. A instância do SQL Server passiva não serve dados do SQL Server aos clientes nem executa cargas de trabalho ativas do SQL Server. É apenas utilizada para sincronizar com o servidor primário e manter a base de dados passiva num estado de reserva semiativa. Se estiver a servir dados, tais como relatórios a clientes que executam cargas de trabalho do SQL Server ativos, ou a realizar qualquer trabalho que não seja especificado nos termos do produto, deve ser uma instância de Servidor SQL licenciada paga. A seguinte atividade é permitida na instância secundária: verificações da consistência da base de dados ou CheckDB, cópias de segurança completas, cópias de segurança do registo de transações e monitorização dos dados de utilização dos recursos. Também pode executar a instância de recuperação após desastre primária correspondente por breves períodos de teste de recuperação após desastre a cada 90 dias. 
   1. A licença de Servidor SQL ativa é coberta pela Software Assurance e permite **uma** instância passiva de sql server secundário, com até a mesma quantidade de cálculo que o servidor ativo licenciado, apenas. 
   1. O VM do servidor SQL secundário utiliza a licença [de Recuperação de Desastres](business-continuity-high-availability-disaster-recovery-hadr-overview.md#free-dr-replica-in-azure) no portal Azure.
   
1. **O que é considerado uma instância passiva?**

   A instância do SQL Server passiva não serve dados do SQL Server aos clientes nem executa cargas de trabalho ativas do SQL Server. É apenas utilizada para sincronizar com o servidor primário e manter a base de dados passiva num estado de reserva semiativa. Se estiver a servir dados, como relatórios a clientes que executam cargas de trabalho do SQL Server ativas ou se estiver a realizar qualquer trabalho que não seja o especificado nos termos do produto, deverá utilizar uma instância do SQL Server licenciada paga. A seguinte atividade é permitida na instância secundária: verificações da consistência da base de dados ou CheckDB, cópias de segurança completas, cópias de segurança do registo de transações e monitorização dos dados de utilização dos recursos. Também pode executar a instância de recuperação após desastre primária correspondente por breves períodos de teste de recuperação após desastre a cada 90 dias.
   

1. **Que cenários podem utilizar o benefício de Recuperação após Desastre (DR)?**

   O [guia de licenciamento](https://aka.ms/sql2019licenseguide) fornece cenários em que o Benefício de Recuperação de Desastres pode ser utilizado. Veja os Termos de Produto e fale com os seus contactos de licenciamento ou gestor de conta para obter mais informações.

1. **Que subscrições suportam o benefício de Recuperação após Desastre (DR)?**

   Programas abrangentes que oferecem direitos de subscrição equivalentes ao Software Assurance como um benefício fixo que suporta o benefício DR. Inclui, mas não se limita a, o Valor Aberto (OV), Subscrição de Valor Aberto (OVS), Acordo Empresarial (EA), Subscrição do Acordo Empresarial (EAS) e Inscrição no Servidor e Na nuvem (SCE). Consulte os termos do [produto](https://www.microsoft.com/licensing/product-licensing/products) e fale com os seus contactos de licenciamento ou gestor de conta para obter mais informações. 

   
 ## <a name="extension"></a>Extensão

1. **Registar o meu VM com a nova extensão do Agente IAAS SQL trará custos adicionais?**

   N.º A extensão do Agente IAAS SQL apenas permite uma gestão adicional para o SQL Server em Azure VM sem custos adicionais. 

1. **A extensão do Agente IAAS SQL está disponível para todos os clientes?**
 
   Sim, desde que o SQL Server VM tenha sido implantado na nuvem pública utilizando o modelo De Gestor de Recursos, e não o modelo clássico. Todos os outros clientes podem registar-se com a nova extensão do Agente IAAS SQL. No entanto, apenas os clientes com o benefício [de Garantia de Software](https://www.microsoft.com/licensing/licensing-programs/software-assurance-default?activetab=software-assurance-default-pivot%3aprimaryr3) podem utilizar a sua própria licença ativando o [Azure Hybrid Benefit (AHB)](https://azure.microsoft.com/pricing/hybrid-benefit/) num SQL Server VM. 

1. **O que acontece com o recurso de extensão _(Microsoft.SqlVirtualMachine)_ se o recurso VM for movido ou largado?** 

   Quando o recurso Microsoft.Compute/VirtualMachine é largado ou movido, então o recurso Microsoft.SqlVirtualMachine associado é notificado para replicar assíncronamente a operação.

1. **O que acontece com o VM se o recurso de extensão (_Microsoft.SqlVirtualMachine)_ for eliminado?**

    O recurso Microsoft.Compute/VirtualMachine não é afetado quando o recurso Microsoft.SqlVirtualMachine é eliminado. No entanto, as alterações de licenciamento voltarão a ser desafinada na fonte de imagem original. 

1. **É possível registar VMs de servidor SQL auto-implantados com a extensão sql IaaS Agent?**

    Sim. Se implementou o SQL Server a partir do seu próprio meio de comunicação e instalou a extensão SQL IaaS, pode registar o seu SQL Server VM com a extensão para obter os benefícios de gestão fornecidos pela extensão SQL IaaS.    


## <a name="administration"></a>Administração

1. **Posso instalar uma segunda instância do SQL Server no mesmo VM? Posso alterar as funcionalidades instaladas da instância padrão?**

   Sim. O meio de instalação do SQL Server está localizado numa pasta na unidade **C.** Executar **Setup.exe** a partir desse local para adicionar novas instâncias do SQL Server ou para alterar outras funcionalidades instaladas do SQL Server na máquina. Note que algumas funcionalidades, tais como cópia de segurança automatizada, patching automatizado e integração de cofre de chave Azure, apenas operam contra a instância padrão, ou um caso nomeado que foi configurado corretamente (Ver Pergunta 3). Os clientes que utilizam [a Software Assurance através do Azure Hybrid Benefit](licensing-model-azure-hybrid-benefit-ahb-change.md) ou do modelo de licenciamento **pay-as-you-go** podem instalar várias instâncias do SQL Server na máquina virtual sem incorrer em custos de licenciamento extra. Casos adicionais do SQL Server podem forçar os recursos do sistema a menos que configurados corretamente. 

1. **Qual é o número máximo de ocorrências num VM?**
   O SQL Server 2012 para o SQL Server 2019 pode suportar [50 instâncias](/sql/sql-server/editions-and-components-of-sql-server-version-15#RDBMSSP) num servidor autónomo. Este é o mesmo limite, independentemente de em Azure no local. Consulte [as melhores práticas](performance-guidelines-best-practices.md#multiple-instances) para aprender a preparar melhor o seu ambiente. 

1. **Posso desinstalar a instância predefinida do SQL Server?**

   Sim, mas há considerações a ter em conta. Em primeiro lugar, a faturação associada ao SQL Server pode continuar a ocorrer dependendo do modelo de licença para o VM. Em segundo lugar, tal como indicado na resposta anterior, existem funcionalidades que dependem da extensão do [agente iaaS do servidor SQL](sql-server-iaas-agent-extension-automate-management.md). Se desinstalar a instância padrão sem remover também a extensão IaaS, a extensão continua a procurar a instância padrão e pode gerar erros de registo de eventos. Estes erros são das duas fontes seguintes: **Microsoft SQL Server Credential Management** e **Microsoft SQL Server IaaS Agent**. Um dos erros poderá ser semelhante ao seguinte:

      Ocorreu um erro relacionado com a rede ou específico da instância ao estabelecer uma ligação ao SQL Server. O servidor não foi encontrado ou não está acessível.

   Se decidir desinstalar a instância predefinitiva, também desinstale também a [Extensão do Agente IAAS do Servidor SQL.](sql-server-iaas-agent-extension-automate-management.md) 

1. **Posso usar uma instância nomeada do SQL Server com a extensão IaaS?**
   
   Sim, se a instância nomeada for a única instância no SQL Server, e se a instância por defeito original foi [desinstalada corretamente](sql-server-iaas-agent-extension-automate-management.md#named-instance-support). Se não houver uma instância padrão e existirem múltiplas instâncias nomeadas num único SQL Server VM, a extensão do agente IAAS do SQL Server não será instalada.  

1. **Posso remover o SQL Server e a faturação de licença associada de um SQL Server VM?**

   Sim, mas terá de tomar medidas adicionais para evitar ser cobrado pela sua instância sql Server, conforme descrito na [orientação de Preços.](pricing-guidance.md) Se pretender remover completamente a instância do SQL Server, pode migrar para outro VM Azure sem o SQL Server pré-instalado no VM e eliminar o VM do servidor SQL atual. Se quiser manter o VM mas parar a faturação do SQL Server, siga estes passos: 

   1. Ressou todos os seus dados, incluindo bases de dados do sistema, se necessário. 
   1. Desinstale completamente o SQL Server, incluindo a extensão SQL IaaS (se estiver presente).
   1. Instale a [edição gratuita do SQL Express.](https://www.microsoft.com/sql-server/sql-server-downloads)
   1. Registe-se com a extensão sql IaaS Agent no [modo leve](sql-agent-extension-manually-register-single-vm.md).
   1. [Altere a edição do SQL Server](change-sql-server-edition.md#change-edition-in-portal) no [portal Azure](https://portal.azure.com) para Express para parar a faturação.  
   1. (opcional) Desative o serviço Express SQL Server desativando o arranque do serviço. 

1. **Posso utilizar o portal do Azure para gerir várias instâncias na mesma VM?**

   Não. A gestão do portal é fornecida pela extensão do Agente IAAS SQL, que se baseia na extensão do Agente IAAS do SQL Server. Como tal, as mesmas limitações aplicam-se à extensão que a extensão. O portal pode gerir apenas uma instância padrão, ou uma instância nomeada, desde que seja configurada corretamente. Para mais informações, consulte [a extensão do Agente IAAS do SqL Server](sql-server-iaas-agent-extension-automate-management.md) 


## <a name="updating-and-patching"></a>Atualização e remendação

1. **Como posso mudar para uma versão/edição diferente do SQL Server num VM Azure?**

   Os clientes podem alterar a sua versão/edição do SQL Server com o suporte de dados de instalação que contém a versão ou a edição pretendida do SQL Server. Uma vez a edição alterada, utilize o portal do Azure para alterar a propriedade de edição da VM para refletir com precisão a faturação da VM. Para mais informações, consulte [a edição de alteração de um SQL Server VM](change-sql-server-edition.md). Não existem diferenças a nível de faturação para versões diferentes do SQL Server, quando a versão do SQL Server for alterada, não é necessária mais nenhuma ação.

1. **Onde posso obter o suporte de dados de configuração para alterar a edição ou a versão do SQL Server?**

   Os clientes que tiverem o [Software Assurance](https://www.microsoft.com/licensing/licensing-programs/software-assurance-default) poderão obter o suporte de dados de instalação no [Centro de Licenciamento em Volume](https://www.microsoft.com/Licensing/servicecenter/default.aspx). Os clientes que não têm Software Assurance podem utilizar os meios de configuração a partir de uma imagem VM do Azure Marketplace SQL Server que tem a sua edição desejada.
   
1. **Como são aplicados os pacotes de serviços e atualizações numa VM do SQL Server?**

   As máquinas virtuais dão-lhe controlo sobre o computador anfitrião, incluindo o momento e a forma como aplica atualizações. No que respeita o sistema operativo, pode aplicar atualizações do Windows manualmente ou pode ativar um serviço de agendamento denominado [Aplicação de Patches Automatizada](automated-patching.md). A Aplicação de Patches Automatizada instala todas as atualizações marcadas como importantes, incluindo atualizações do SQL Server nessa categoria. As outras atualizações opcionais do SQL Server têm de ser instaladas manualmente.

1. **Posso atualizar a minha instância SQL Server 2008 / 2008 R2 depois de a registar com a extensão do Agente IAAS SQL?**

   Se o SISTEMA for o Windows Server 2008 R2 ou mais tarde, sim. Pode utilizar qualquer suporte de configuração para atualizar a versão e edição do SQL Server e, em seguida, pode atualizar o seu modo de [extensão SQL IaaS](sql-server-iaas-agent-extension-automate-management.md#management-modes)) de _nenhum agente_ para _o máximo_. Ao fazê-lo, você terá acesso a todos os benefícios da extensão SQL IaaS, como a gestão do portal, backups automatizados e patching automatizado. Se a versão OS for o Windows Server 2008, apenas o modo NoAgent é suportado. 

1. **Como posso obter atualizações de segurança alargada gratuitas para o fim de suporte das instâncias do SQL Server 2008 e do SQL Server 2008 R2?**

   Pode obter [atualizações de segurança estendidas gratuitas](sql-server-2008-extend-end-of-support.md) movendo o seu SQL Server como está para uma máquina virtual Azure. Para obter mais informações, veja [end of support options](/sql/sql-server/end-of-support/sql-server-end-of-life-overview) (opções de fim de suporte). 
  
   

## <a name="general"></a>Geral

1. **SqL Server falha casos de cluster (FCI) suportados em VMs Azure?**

   Sim. Pode instalar uma instância de cluster de falha usando [ações de ficheiros premium (PFS)](failover-cluster-instance-premium-file-share-manually-configure.md) ou [espaços de armazenamento diretos (S2D)](failover-cluster-instance-storage-spaces-direct-manually-configure.md) para o subsistema de armazenamento. As ações de ficheiros premium fornecem IOPS e capacidades de produção que irão atender às necessidades de muitas cargas de trabalho. Para cargas de trabalho intensivas em IO, considere a utilização de espaços de armazenamento diretamente baseados em prémios manged ou ultra-discos. Em alternativa, pode utilizar soluções de agrupamento ou armazenamento de terceiros, conforme descrito na [alta disponibilidade e recuperação de desastres para o SQL Server em Azure Virtual Machines](business-continuity-high-availability-disaster-recovery-hadr-overview.md#azure-only-high-availability-solutions).

   > [!IMPORTANT]
   > Neste momento, a extensão _completa_ do [agente do SQL Server IaaS](sql-server-iaas-agent-extension-automate-management.md) não é suportada para o SQL Server FCI em Azure. Recomendamos que desinstale a extensão _completa_ dos VMs que participam no FCI e instale a extensão em modo _leve._ Esta extensão suporta funcionalidades, tais como Cópia de Segurança Automatizada e Patching e algumas funcionalidades do portal para o SQL Server. Estas funcionalidades não funcionarão para VMs do SQL Server depois de o agente _completo_ ser desinstalado.

1. **Qual é a diferença entre os VMs do SQL Server e o serviço sql Database?**

   Conceptualmente, executar o SQL Server numa máquina virtual Azure não é assim tão diferente de executar o SQL Server num centro de dados remoto. Em contraste, [a Azure SQL Database](../../database/sql-database-paas-overview.md) oferece base de dados como serviço. Com base de dados SQL, não tem acesso às máquinas que acolhem as suas bases de dados. Para uma comparação completa, consulte [Escolha uma opção de servidor SQL em nuvem: Base de dados Azure SQL (PaaS) ou SQL Server em VMs Azure (IaaS)](../../azure-sql-iaas-vs-paas-what-is-overview.md).

1. **Como posso instalar ferramentas SQL Data no meu Azure VM?**

    Descarregue e instale as ferramentas de dados SQL a partir de Ferramentas de [Dados do Servidor Microsoft SQL - Business Intelligence for Visual Studio 2013](https://www.microsoft.com/download/details.aspx?id=42313).

1. **As transações distribuídas com MSDTC são suportadas em VMs do SQL Server?**
   
    Sim. O DTC local é suportado para SQL Server 2016 SP2 e maior. No entanto, as aplicações devem ser testadas ao utilizar grupos de disponibilidade Always On, uma vez que as transações a bordo durante uma falha falharão e devem ser novamente julgadas. O Clustered DTC está disponível a partir do Windows Server 2019. 
    
1. **A máquina virtual Azure SQL move-se ou armazena os dados dos clientes para fora da região?**

   N.º De facto, a máquina virtual Azure SQL e a Extensão do Agente SQL IaaS não armazenam quaisquer dados do cliente.

## <a name="sql-server-iaas-agent-extension"></a>Extensão do Agente IaaS do SQL Server

1. **Devo registar o meu SQL Server VM a partir de uma imagem do SQL Server no Azure Marketplace?**

   N.º A Microsoft regista automaticamente VMs a partir das imagens do SQL Server no Azure Marketplace. O registo com a extensão só é necessário se o VM *não* tiver sido aprovisionado a partir das imagens do SQL Server no Azure Marketplace e no SQL Server se autoinstalar.

1. **A extensão do Agente IAAS SQL está disponível para todos os clientes?** 

   Sim. Os clientes devem registar os seus VMs sql server com a extensão se não utilizarem uma imagem sql Server do Azure Marketplace e, em vez disso, o SqL Server auto-instalado, ou se trouxerem o seu VHD personalizado. Os VMs detidos por todos os tipos de subscrições (Direct, Enterprise Agreement e Cloud Solution Provider) podem registar-se com a extensão do Agente IAAS SQL.

1. **Qual é o modo de gestão predefinido ao registar-se com a extensão do Agente IAAS SQL?**

   O modo de gestão predefinido quando se regista com a extensão SQL IaaS Agent é *leve*. Se a propriedade de gestão do SQL Server não estiver definida quando se registar com a extensão, o modo será definido como leve e o seu serviço SQL Server não reiniciará. Recomenda-se registar-se primeiro com a extensão SQL IaaS Agent em modo leve e, em seguida, atualizar para a totalidade durante uma janela de manutenção. Da mesma forma, a gestão predefinitiva também é leve quando se utiliza a [função de registo automático](sql-agent-extension-automatic-registration-all-vms.md).

1. **Quais são os pré-requisitos para se registar com a extensão do Agente IAAS SQL?**

   Não existem pré-requisitos para registar-se com a extensão sql IaaS Agent além de ter o SQL Server instalado no VM. Note que se a extensão do agente SQL IaaS for instalada em pleno modo, o serviço SQL Server reiniciará, assim o recomendará durante uma janela de manutenção.

1. **Registar-se com a extensão do Agente IAAS SQL vai instalar um agente no meu VM?**

   Sim, registar-se com a extensão sql IaaS Agent em modo de gestão total instala um agente no VM. Registar-se em modo leve ou NoAgent não. 

   Registar-se com a extensão SQL IaaS Agent em modo leve apenas copia as *binárias* de extensão sql IaaS Agent para o VM, não instala o agente. Estes binários são então utilizados para instalar o agente quando o modo de gestão é atualizado para o máximo.


1. **O registo com a extensão do SqL IaaS Agent reiniciará o SQL Server no meu VM?**

   Depende do modo especificado durante o registo. Se for especificado o modo "NoAgent" leve ou noAgent, o serviço SQL Server não será reiniciado. No entanto, especificar o modo de gestão como completo fará com que o serviço SQL Server reinicie. A função de registo automático regista os seus VMs do SQL Server em modo leve, a menos que a versão do Windows Server seja 2008, caso em que o SQL Server VM será registado no modo NoAgent. 

1. **Qual é a diferença entre os modos de gestão leve e NoAgent ao registar-se com a extensão do Agente IAAS SQL?** 

   O modo de gestão NoAgent é o único modo de gestão disponível para SQL Server 2008 e SQL Server 2008 R2 no Windows Server 2008. Para todas as versões posteriores do Windows Server, os dois modos de gestão disponíveis são leves e cheios. 

   O modo NoAgent requer que a versão e as propriedades de edição do SQL Server sejam definidas pelo cliente. O modo leve consulta o VM para encontrar a versão e edição da instância SQL Server.

1. **Posso registar-me com a extensão do Agente IAAS SQL sem especificar o tipo de licença SQL Server?**

   N.º O tipo de licença SQL Server não é uma propriedade opcional quando se regista com a extensão do Agente IAAS SQL. Tem de definir o tipo de licença SQL Server como pay-as-you-go ou Azure Hybrid Benefit ao registar-se com a extensão sql IaaS Agent em todos os modos de gestão (NoAgent, leve e completo). Se tiver alguma das versões gratuitas do SQL Server instaladas, como a edição de Desenvolvimento ou Avaliação, deve registar-se com o licenciamento pay-as-you-go. O Azure Hybrid Benefit só está disponível para versões pagas do SQL Server, como edições Enterprise e Standard.

1. **Posso atualizar a extensão IAAS do SQL Server do modo NoAgent para o modo completo?**

   N.º A atualização do modo de gestão para o modo de gestão para o modo NoAgent ou para o modo NoAgent não está disponível para o modo NoAgent. Esta é uma limitação técnica do Windows Server 2008. Terá de atualizar o SISTEMA primeiro para o Windows Server 2008 R2 ou superior, e depois poderá fazer o upgrade para o modo de gestão completa. 

1. **Posso atualizar a extensão IAAS do SQL Server do modo leve para o modo completo?**

   Sim. A atualização do modo de gestão de peso para cheio é suportada através do Azure PowerShell ou do portal Azure. Isto irá desencadear um reinício do serviço SQL Server.

1. **Posso reduzir a extensão IAAS do SQL Server de modo completo para modo NoAgent ou modo de gestão leve?**

   N.º A redução do modo de gestão de extensão SQL Server IaaS não é suportada. O modo de gestão não pode ser desclassificado do modo completo para o modo leve ou noAgent, e não pode ser desclassificado do modo leve para o modo NoAgent. 

   Para alterar o modo de gestão da plena gestibilidade, [desagrega](sql-agent-extension-manually-register-single-vm.md#unregister-from-extension) o SQL Server VM a partir da extensão SQL IaaS Agent, largando o _recurso_ da máquina virtual SQL e reregistando novamente o SQL Server VM com a extensão SQL IaaS Agent novamente num modo de gestão diferente.

1. **Posso registar-me com a extensão do Agente IAAS sql do portal Azure?**

   N.º O registo com a extensão do Agente IAAS SQL não está disponível no portal Azure. Registar-se com a extensão sql IaaS Agent só é suportado com o Azure CLI ou Azure PowerShell. 

1. **Posso registar um VM com a extensão SQL IaaS Agent antes da instalação do SQL Server?**

   N.º Um VM deve ter pelo menos uma instância SQL Server (Database Engine) para registar com sucesso com a extensão do Agente IAAS SQL. Se não houver nenhuma instância do SQL Server no VM, o novo recurso Microsoft.SqlVirtualMachine estará num estado falhado.

1. **Posso registar um VM com a extensão do Agente IAAS SQL se existirem várias instâncias do SQL Server?**

   Sim, desde que haja uma instância padrão no VM. A extensão do Agente IAAS SQL registará apenas uma instância do SQL Server (Database Engine). A extensão do Agente SQL IaaS registará a instância padrão do SQL Server no caso de múltiplas instâncias.

1. **Posso registar uma instância de cluster de falha do SQL Server com a extensão do Agente IAAS SQL?**

   Sim. Sql Server falha casos de cluster num Azure VM pode ser registado com a extensão sql IaaS Agent em modo leve. No entanto, as instâncias de cluster de failover do SQL Server não podem ser atualizadas para o modo de gestão completa.

1. **Posso registar o meu VM com a extensão do Agente IAAS SQL se um grupo de disponibilidade Always On estiver configurado?**

   Sim. Não existem restrições ao registo de uma instância do SQL Server num VM Azure com a extensão SQL IaaS Agent se estiver a participar numa configuração do grupo de disponibilidade Always On.

1. **Qual o custo para o registo com a extensão do Agente IAAS SQL, ou com o upgrade para o modo de gestão completa?**

   Nenhum. Não existe qualquer taxa associada ao registo com a extensão do Agente IAAS SQL, ou à utilização de qualquer um dos três modos de gestão. Gerir o seu SQL Server VM com a extensão é completamente gratuito. 

1. **Qual é o impacto de desempenho da utilização dos diferentes modos de gestão?**

   Não há impacto na utilização dos modos de gestão *NoAgent* e *leves.* Existe um impacto mínimo ao utilizar o modo de gestão *total* de dois serviços instalados no SISTEMA. Estes podem ser monitorizados através do gestor de tarefas e vistos na consola de Serviços Windows incorporada. 

   Os dois nomes de serviço são:
   - `SqlIaaSExtensionQuery` (Nome de exibição - `Microsoft SQL Server IaaS Query Service` )
   - `SQLIaaSExtension` (Nome de exibição - `Microsoft SQL Server IaaS Agent` )

1. **Como retiro a extensão?**

   Remova a extensão [não registrando](sql-agent-extension-manually-register-single-vm.md#unregister-from-extension) o SQL Server VM da extensão SQL IaaS Agent. 

## <a name="resources"></a>Recursos

**VMs do Windows:**

* [Visão geral do SQL Server num VM do Windows](sql-server-on-azure-vm-iaas-what-is-overview.md)
* [Provisão SQL Server em um VM Windows](create-sql-vm-portal.md)
* [Migrar uma base de dados para o SQL Server num VM Azure](migrate-to-vm-from-sql-server.md)
* [Alta disponibilidade e recuperação de desastres para o servidor SQL em máquinas virtuais Azure](business-continuity-high-availability-disaster-recovery-hadr-overview.md)
* [Melhores práticas de desempenho para SQL Server em Azure Virtual Machines](performance-guidelines-best-practices.md)
* [Padrões de aplicação e estratégias de desenvolvimento para servidor SQL em máquinas virtuais Azure](application-patterns-development-strategies.md)

**Linux VMs:**

* [Visão geral do SQL Server num Linux VM](../linux/sql-server-on-linux-vm-what-is-iaas-overview.md)
* [Provisão SQL Server em um Linux VM](../linux/sql-vm-create-portal-quickstart.md)
* [FAQ (Linux)](../linux/frequently-asked-questions-faq.md)
* [SqL Server na documentação do Linux](/sql/linux/sql-server-linux-overview)
