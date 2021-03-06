---
title: Problemas de resolução de problemas do Windows Problemas de Agente de Ambiente de Trabalho Virtual - Azure
description: Como resolver problemas comuns de agente e conectividade.
author: Sefriend
ms.topic: troubleshooting
ms.date: 12/16/2020
ms.author: sefriend
manager: clarkn
ms.openlocfilehash: 2f321413a275676d0abb1a075ba958885ffcd821
ms.sourcegitcommit: c2a41648315a95aa6340e67e600a52801af69ec7
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/06/2021
ms.locfileid: "106505030"
---
# <a name="troubleshoot-common-windows-virtual-desktop-agent-issues"></a>Resolução de problemas problemas comuns do Windows Virtual Desktop Agent

O Windows Virtual Desktop Agent pode causar problemas de ligação devido a vários fatores:
   - Um erro no corretor que faz com que o agente pare o serviço.
   - Problemas com atualizações.
   - Problemas com a instalação durante a instalação do agente, o que interrompe a ligação ao anfitrião da sessão.

Este artigo irá guiá-lo através de soluções para estes cenários comuns e como abordar questões de conexão.

>[!NOTE]
>Para problemas de resolução de problemas relacionados com a conectividade da sessão e o agente de ambiente de trabalho virtual do Windows, recomendamos que reveja os registos de eventos na   >  Aplicação **de Registos do Windows** do Espectador de Eventos  >  . Procure eventos que tenham uma das seguintes fontes para identificar o seu problema:
>
>- WVD-Agent
>- WVD-Agente-Atualização
>- RDAgentBootLoader
>- MsiInstaller

## <a name="error-the-rdagentbootloader-andor-remote-desktop-agent-loader-has-stopped-running"></a>Erro: O carregador de artigos de secretária RDAgentBootLoader e/ou Remote Desktop Agent Loader deixou de funcionar

Se estiver a ver algum dos seguintes problemas, isto significa que o carregador de arranque, que carrega o agente, não foi capaz de instalar corretamente o agente e o serviço de agente não está a funcionar:
- **RDAgentBootLoader** está parado ou não está a funcionar.
- Não existe nenhum estado para **o "Remote Desktop Agent Loader".**

Para resolver este problema, inicie o carregador de arranque RDAgent:

1. Na janela Serviços, clique à direita **no Carregador de Agente de Ambiente de Trabalho Remoto**.
2. Selecione **Iniciar**. Se esta opção estiver cinzenta para si, não tem permissões de administrador e terá de as fazer iniciar o serviço.
3. Aguarde 10 segundos e, em seguida, clique com o botão **direito Do Carregador de Agente de Ambiente de Trabalho Remoto**.
4. Selecione **Refresh**.
5. Se o serviço parar depois de ter começado e o refrescar, poderá ter uma falha de registo. Para mais informações, consulte [INVALID_REGISTRATION_TOKEN.](#error-invalid_registration_token)

## <a name="error-invalid_registration_token"></a>Erro: INVALID_REGISTRATION_TOKEN

Aceda à aplicação de registos **do Windows do espectador** de  >  **eventos.**  >   Se vir um evento com o ID 3277, que diz **INVALID_REGISTRATION_TOKEN** na descrição, o sinal de inscrição que você tem não é reconhecido como válido.

Para resolver esta questão, crie um símbolo de registo válido:

1. Para criar um novo token de inscrição, siga os passos na Secção Gerar uma nova chave de registo para a secção [VM.](#step-3-generate-a-new-registration-key-for-the-vm)
2. Abra o Editor de Registo. 
3. Vá para **HKEY_LOCAL_MACHINE**  >  **SOFTWARE**  >  **Microsoft**  >  **RDInfraAgent**.
4. Selecione **IsRegistered**. 
5. Nos **dados** de valor: caixa de entrada, tipo **0** e selecione **Ok**. 
6. Selecione **RegistrationToken**. 
7. Nos **dados** do Valor: caixa de entrada, cole o token de registo a partir do passo 1. 

   > [!div class="mx-imgBorder"]
   > ![Screenshot de IsRegistered 0](media/isregistered-token.png)

8. Abra uma linha de comandos como administrador.
9. Introduza **a paragem líquida RDAgentBootLoader**. 
10. Introduza **o arranque da rede RDAgentBootLoader**. 
11. Abra o Editor de Registo.
12. Vá para **HKEY_LOCAL_MACHINE**  >  **SOFTWARE**  >  **Microsoft**  >  **RDInfraAgent**.
13. Verifique se **o IsRegistered** está definido para 1 e não há nada na coluna de dados para **o RegistrationToken**. 

   > [!div class="mx-imgBorder"]
   > ![Screenshot de IsRegistered 1](media/isregistered-registry.png)

## <a name="error-agent-cannot-connect-to-broker-with-invalid_form"></a>Erro: O agente não pode ligar-se ao corretor com INVALID_FORM

Aceda à aplicação de registos **do Windows do espectador** de  >  **eventos.**  >   Se vires um evento com o ID 3277 que diga "INVALID_FORM" na descrição, algo correu mal com a comunicação entre o agente e o corretor. O agente não pode ligar-se ao corretor ou chegar a um URL específico devido a determinadas definições de firewall ou DNS.

Para resolver este problema, verifique se pode contactar a BrokerURI e a BrokerURIGlobal:
1. Abra o Editor de Registos. 
2. Vá para **HKEY_LOCAL_MACHINE**  >  **SOFTWARE**  >  **Microsoft**  >  **RDInfraAgent**. 
3. Tome nota dos valores para **BrokerURI** e **BrokerURIGlobal**.

   > [!div class="mx-imgBorder"]
   > ![Screenshot do corretor uri e corretor uri global](media/broker-uri.png)

 
4. Abra um navegador e vá para *\<BrokerURI\> api/saúde.* 
   - Certifique-se de que utiliza o valor do passo 3 no **BrokerURI**. No exemplo desta secção, <https://rdbroker-g-us-r0.wvd.microsoft.com/api/health> seria.
5. Abra outro separador no navegador e vá para *\<BrokerURIGlobal\> api/saúde.* 
   - Certifique-se de que utiliza o valor do passo 3 na ligação **BrokerURIGlobal.** No exemplo desta secção, <https://rdbroker.wvd.microsoft.com/api/health> seria.
6. Se a rede não estiver a bloquear a ligação do corretor, ambas as páginas carregarão com sucesso e mostrarão uma mensagem que diz **"RD Broker is Healthy"** como mostrado nas imagens seguintes. 

   > [!div class="mx-imgBorder"]
   > ![Screenshot do acesso uri do corretor carregado com sucesso](media/broker-uri-web.png)

   > [!div class="mx-imgBorder"]
   > ![Screenshot do corretor carregado com sucesso acesso global uri](media/broker-global.png)
 

7. Se a rede estiver a bloquear a ligação do corretor, as páginas não serão carregadas, como mostrado na imagem seguinte. 

   > [!div class="mx-imgBorder"]
   > ![Screenshot do acesso mal sucedido do corretor carregado](media/unsuccessful-broker-uri.png)

   > [!div class="mx-imgBorder"]
   > ![Screenshot do acesso global de corretor carregado sem sucesso](media/unsuccessful-broker-global.png)

8. Se a rede estiver a bloquear estes URLs, terá de desbloquear os URLs necessários. Para obter mais informações, consulte [a Lista de URL Necessária.](safe-url-list.md)
9. Se isto não resolver o seu problema, certifique-se de que não tem nenhuma política de grupo com cifras que bloqueiem a ligação do agente ao corretor. O Windows Virtual Desktop utiliza as mesmas cifras TLS 1.2 que [a Porta Frontal Azure](../frontdoor/front-door-faq.yml#what-are-the-current-cipher-suites-supported-by-azure-front-door-). Para mais informações, consulte [a Connection Security](network-connectivity.md#connection-security).

## <a name="error-3703"></a>Erro: 3703

Aceda à aplicação de registos **do Windows do espectador** de  >  **eventos.**  >   Se vir um evento com iD 3703 que diz "URL RD Gateway: não está acessível" na descrição, o agente não consegue chegar aos URLs de gateway. Para ligar com sucesso ao anfitrião da sessão e permitir que o tráfego de rede para estes pontos finais para contornar as restrições, tem de desbloquear os URLs da [Lista de URL Requerida.](safe-url-list.md) Além disso, certifique-se de que as definições de firewall ou proxy não bloqueiam estes URLs. Desbloqueando estes URLs é necessário para utilizar o Windows Virtual Desktop.

Para resolver este problema, verifique se as definições de firewall e/ou DNS não estão a bloquear estes URLs:
1. [Utilize o Azure Firewall para proteger as implementações do Windows Virtual Desktop.](../firewall/protect-windows-virtual-desktop.md)
2. Configure as [definições de DNS da Azure Firewall](../firewall/dns-settings.md).

## <a name="error-3019"></a>Erro: 3019

Aceda à aplicação de registos **do Windows do espectador** de  >  **eventos.**  >   Se vir um evento com iD 3019, isto significa que o agente não pode chegar aos URLs de transporte de tomada web. Para ligar com sucesso ao anfitrião da sessão e permitir que o tráfego de rede contorne estas restrições, tem de desbloquear os URLs listados na [lista de URL requerida.](safe-url-list.md) Trabalhe com a equipa de Networking Azure para garantir que as definições de firewall, proxy e DNS não estão a bloquear estes URLs. Também pode verificar os registos de rastreios de rede para identificar onde o serviço de ambiente de trabalho virtual do Windows está bloqueado. Se abrir um pedido de apoio para este problema em particular, certifique-se de anexar os registos de rastreios de rede ao pedido.

## <a name="error-installationhealthcheckfailedexception"></a>Erro: InstalaçãoHealthCheckFailedException

Aceda à aplicação de registos **do Windows do espectador** de  >  **eventos.**  >   Se vir um evento com o ID 3277 que diz "InstallationHealthCheckFailedException" na descrição, isso significa que o ouvinte da pilha não está a funcionar porque o servidor do terminal alterou a chave de registo para o ouvinte da pilha.

Para resolver este problema:
1. Verifique se [o ouvinte da pilha está a funcionar.](#error-stack-listener-isnt-working-on-windows-10-2004-vm)
2. Se o ouvinte da pilha não estiver a funcionar, [desinstale manualmente e reinstale o componente da pilha](#error-vms-are-stuck-in-unavailable-or-upgrading-state).

## <a name="error-endpoint_not_found"></a>Erro: ENDPOINT_NOT_FOUND

Aceda à aplicação de registos **do Windows do espectador** de  >  **eventos.**  >   Se vir um evento com o ID 3277 que diga "ENDPOINT_NOT_FOUND" na descrição, isso significa que o corretor não conseguiu encontrar um ponto final para estabelecer uma ligação com. Esta questão de ligação pode acontecer por uma das seguintes razões:

- Não há VMs na sua piscina de anfitriões.
- Os VMs na sua piscina de anfitriões não estão ativos
- Todos os VMs na sua piscina de anfitriões excederam o limite máximo de sessão
- Nenhum dos VMs na sua piscina de anfitrião tem o serviço de agente a funcionar neles.

Para resolver este problema:

1. Certifique-se de que o VM está ligado e não foi removido da piscina hospedeira.
2. Certifique-se de que o VM não excedeu o limite máximo de sessão.
3. Certifique-se de que o serviço de [agente está a funcionar](#error-the-rdagentbootloader-andor-remote-desktop-agent-loader-has-stopped-running) e que o ouvinte da pilha está a [funcionar](#error-stack-listener-isnt-working-on-windows-10-2004-vm).
4. Certifique-se de que [o agente pode ligar-se ao corretor](#error-agent-cannot-connect-to-broker-with-invalid_form).
5. Certifique-se de que [o seu VM tem um token de registo válido.](#error-invalid_registration_token)
6. Certifique-se de que o sinal de [registo VM não expirou.](faq.md#how-often-should-i-turn-my-vms-on-to-prevent-registration-issues) 

## <a name="error-installmsiexception"></a>Erro: Instalar Adiscepção

Aceda à aplicação de registos **do Windows do espectador** de  >  **eventos.**  >   Se vir um evento com o ID 3277, que diz **InstallMsiException** na descrição, o instalador já está a executar outra aplicação enquanto está a tentar instalar o agente, ou uma política está a bloquear o msiexec.exe programa de funcionamento.

Para resolver esta questão, desative a seguinte política:
   - Desligue o Instalador do Windows  
      - Caminho da categoria: Configuração do computador\Modelos administrativos\Componentes do Windows\Instalador windows
   
>[!NOTE]
>Esta não é uma lista completa de políticas, apenas aquelas que conhecemos atualmente.

Para desativar uma política:
1. Abra uma linha de comandos como administrador.
2. Entrar e executar **rsop.msc**.
3. Na janela **resultado do conjunto de política** que aparece, vá para o caminho da categoria.
4. Selecione a política.
5. Selecione **Desativado**.
6. Selecione **Aplicar**.   

   > [!div class="mx-imgBorder"]
   > ![Screenshot da política do instalador do Windows em conjunto de política resultante](media/gpo-policy.png)

## <a name="error-win32exception"></a>Erro: Win32Exception

Aceda à aplicação de registos **do Windows do espectador** de  >  **eventos.**  >   Se vir um evento com o ID 3277, que diz **InstallMsiException** na descrição, uma política está a bloquear cmd.exe de lançamento. Bloquear este programa impede-o de executar a janela da consola, que é o que precisa de utilizar para reiniciar o serviço sempre que o agente atualiza.

Para resolver esta questão, desative a seguinte política:
   - Impedir o acesso à linha de comandos   
      - Caminho da categoria: Configuração do utilizador\Modelos administrativos\Sistema
    
>[!NOTE]
>Esta não é uma lista completa de políticas, apenas aquelas que conhecemos atualmente.

Para desativar uma política:
1. Abra uma linha de comandos como administrador.
2. Entrar e executar **rsop.msc**.
3. Na janela **resultado do conjunto de política** que aparece, vá para o caminho da categoria.
4. Selecione a política.
5. Selecione **Desativado**.
6. Selecione **Aplicar**.   

## <a name="error-stack-listener-isnt-working-on-windows-10-2004-vm"></a>Erro: O ouvinte da Stack não está a trabalhar no Windows 10 2004 VM

Faça **qwinsta** no seu pedido de comando e tome nota do número de versão que aparece ao lado **de rdp-sxs**. Se não está a ver os componentes **rdp-tcp** e **RDP-sxs** dizer **Ouvir** ao lado deles ou não aparecer em nada depois de executar **qwinsta**, significa que há um problema de pilha. As atualizações da Stack são instaladas juntamente com as atualizações do agente, e quando esta instalação correr mal, o Windows Virtual Desktop Listener não funcionará.

Para resolver este problema:
1. Abra o Editor de Registo.
2. Vá para **HKEY_LOCAL_MACHINE**  >  **sistema sistema**  >    >    >  **controle terminal de**  >  **servidores WinStations**.
3. No **WinStations** poderá ver várias pastas para diferentes versões de stack, selecione a pasta que corresponde à informação da versão que viu ao executar **qwinsta** no seu Comando Prompt.
4. Encontre **fReverseConnectMode** e certifique-se de que o seu valor de dados é **1**. Certifique-se também de que **a FEnableWinStation** está definida para **1**.

   > [!div class="mx-imgBorder"]
   > ![Screenshot de fReverseConnectMode](media/fenable-2.png)

5. Se **fReverseConnectMode** não estiver definido para **1**, selecione **fReverseConnectMode** e introduza **1** no seu campo de valor. 
6. Se **a FEnableWinStation** não estiver definida para **1**, selecione **fEnableWinStation** e introduza **1** no seu campo de valor.
7. Reinicie o seu VM. 

>[!NOTE]
>Para alterar o modo **fReverseConnectMode** ou **fEnableWinStation** para vários VMs de cada vez, pode fazer uma das duas coisas seguintes:
>
>- Exporte a chave de registo da máquina que já tem a trabalhar e importe-a para todas as outras máquinas que necessitem desta alteração.
>- Crie um objeto de política de grupo (GPO) que define o valor da chave de registo para as máquinas que precisam da mudança.

7. Vá para **HKEY_LOCAL_MACHINE**  >  **Sistema**  >  **sistema**  >  **controlo sistema** sistema  >  **servidor**  >  **de servidores Desconsetos**.
8. No **âmbito do ClusterSettings,** encontre **o SessionDirectoryListener** e certifique-se de que o seu valor de dados é **rdp-sxs...**.
9. Se **o SessionDirectoryListener** não estiver definido para **rdp-sxs...**, terá de seguir os passos na secção [Desinstalar o agente e o carregador de arranque](#step-1-uninstall-all-agent-boot-loader-and-stack-component-programs) para primeiro desinstalar os componentes do agente, do carregador de arranque e da pilha e, em seguida, [reinstalar o agente e o carregador de porta-malas](#step-4-reinstall-the-agent-and-boot-loader). Isto irá reinstalar a pilha lado a lado.

## <a name="error-heartbeat-issue-where-users-keep-getting-disconnected-from-session-hosts"></a>Erro: Problema de batimentos cardíacos onde os utilizadores continuam a ser desligados dos anfitriões da sessão

Se o seu servidor não estiver a captar um batimento cardíaco do serviço de ambiente de trabalho virtual do Windows, terá de alterar o limiar do batimento cardíaco. Isto irá temporariamente atenuar os sintomas do problema, mas não vai corrigir o problema subjacente à rede. Siga as instruções desta secção se um ou mais dos seguintes cenários se aplicarem a si:

- Está a receber um erro **checkSessionHostDomainIsReachableAsync**
- Está a receber um erro **de ConnectionBrokenMissedHeartbeatThreshold**
- Está a receber um erro **de LigaçãoEstablished:UnexpectedNetworkDisconnect**
- Os clientes dos utilizadores continuam a ser desligados
- Os utilizadores continuam a ser desligados dos anfitriões da sessão

Para alterar o limiar do batimento cardíaco:
1. Abra o seu pedido de comando como administrador.
2. Introduza o comando **qwinsta** e ordene.
3. Devem ser apresentados dois componentes de pilha: **RDP-tcp** e **RDP-sxs**. 
   - Dependendo da versão do SISTEMA que está a utilizar, **os rdp-sxs** podem ser seguidos pelo número de construção. Se for, certifique-se de escrever este número para mais tarde.
4. Abra o Editor de Registo.
5. Vá para **HKEY_LOCAL_MACHINE**  >  **sistema sistema**  >    >    >  **controle terminal de**  >  **servidores WinStations**.
6. Em **WinStations,** poderás ver várias pastas para diferentes versões de stack. Selecione a pasta que corresponde ao número da versão do passo 3.
7. Crie um novo DWORD de registo clicando à direita no editor de registo e, em seguida, selecione **Novo**  >  **DWORD (32-bit) Valor**. Ao criar o DWORD, introduza os seguintes valores:
   - Batimento cardíacoInterval: 10000
   - HeartbeatWarnCount: 30 
   - HeartbeatDropCount: 60 
8. Reinicie o seu VM.

>[!NOTE]
>Se alterar o limiar do batimento cardíaco não resolver o seu problema, poderá ter um problema de rede subjacente ao qual terá de contactar a equipa de Networking do Azure.

## <a name="error-downloadmsiexception"></a>Erro: DownloadMsiException

Aceda à aplicação de registos **do Windows do espectador** de  >  **eventos.**  >   Se vir um evento com o ID 3277, que diz **DownloadMsiException** na descrição, não há espaço suficiente no disco para o RDAgent.

Para resolver este problema, faça espaço no seu disco através de:
   - Apagar ficheiros que já não estão no utilizador
   - Aumentar a capacidade de armazenamento do seu VM

## <a name="error-agent-fails-to-update-with-missingmethodexception"></a>Erro: Agente falha na atualização com MissingMethodException

Aceda à aplicação de registos **do Windows do espectador** de  >  **eventos.**  >   Se vir um evento com o ID 3389 que diz "MissingMethodException: Method not found" na descrição, isso significa que o agente virtual do Windows não atualizou com sucesso e reverteu para uma versão anterior. Isto pode ser porque o número de versão da estrutura .NET atualmente instalada nos seus VM é inferior a 4.7.2. Para resolver este problema, é necessário atualizar o .NET para a versão 4.7.2 ou posterior seguindo as instruções de instalação na [documentação .NET Framework](https://support.microsoft.com/topic/microsoft-net-framework-4-7-2-offline-installer-for-windows-05a72734-2127-a15d-50cf-daf56d5faec2).


## <a name="error-vms-are-stuck-in-unavailable-or-upgrading-state"></a>Erro: Os VMs estão presos em estado indisponível ou de upgrade

Abra uma janela PowerShell como administrador e execute o seguinte cmdlet:

```powershell
Get-AzWvdSessionHost -ResourceGroupName <resourcegroupname> -HostPoolName <hostpoolname> | Select-Object *
```

Se o estado indicado para o anfitrião da sessão ou os anfitriões na piscina do anfitrião disser sempre "Indisponível" ou "Upgrade", o agente ou stack não foi instalado com sucesso.

Para resolver este problema, reinstale a pilha lado a lado:
1. Abra uma linha de comandos como administrador.
2. Introduza **a paragem líquida RDAgentBootLoader**. 
3. Vá a Programas e  >  **Funcionalidades de Programas** de Controlo do Painel  >  .
4. Desinstale a versão mais recente do **Remote Desktop Services SxS Network Stack** ou a versão listada em **HKEY_LOCAL_MACHINE**  >  **SYSTEM**  >  **CurrentControlSet**  >  **Control Control Control Terminal**  >  **Server**  >  **WinStations** em **ReverseConnectListener**.
5. Abra uma janela da consola como administrador e vá para **o Programa Ficheiros**  >  **Microsoft RDInfra**.
6. Selecione o componente **SxSStack** ou executar o comando **msiexec /i SxsStack-.msi <version>** para instalar o MSI.
8. Reinicie o seu VM.
9. Volte para o comando e corra o comando **qwinsta.**
10. Verifique se o componente da pilha instalado no passo 6 diz **Ouvir** ao lado.
   - Em caso afirmativo, introduza **o arranque da rede RDAgentBootLoader** na uta de comando e reinicie o seu VM.
   - Caso contrário, terá de voltar a [registar o seu VM e reinstalar o](#your-issue-isnt-listed-here-or-wasnt-resolved) componente do agente.

## <a name="error-connection-not-found-rdagent-does-not-have-an-active-connection-to-the-broker"></a>Erro: Ligação não encontrada: RDAgent não tem uma ligação ativa ao corretor

Os seus VMs podem estar no limite de ligação, por isso o VM não pode aceitar novas ligações.

Para resolver este problema:
   - Diminua o limite máximo de sessão. Isto garante que os recursos são distribuídos de forma mais homogéneca entre os anfitriões das sessões e evitará o esgotamento dos recursos.
   - Aumentar a capacidade de recursos dos VMs.

## <a name="error-operating-a-pro-vm-or-other-unsupported-os"></a>Erro: Operar um Pro VM ou outro SISTEMA Não Suportado

A stack lado a lado só é suportada por SKUs do Windows Enterprise ou do Windows Server SKUs, o que significa que sistemas operativos como o Pro VM não são. Se não tiver um SKU Enterprise ou Server, a stack será instalada no seu VM mas não será ativada, pelo que não a verá aparecer quando executar **qwinsta** na sua linha de comando.

Para resolver este problema, crie um VM que seja o Windows Enterprise ou o Windows Server.
1. Aceda aos [detalhes da máquina virtual](create-host-pools-azure-marketplace.md#virtual-machine-details) e siga os passos 1-12 para configurar uma das seguintes imagens recomendadas:
   - Windows 10 Enterprise multi-sessão, versão 1909
   - Windows 10 Enterprise multi-sessão, versão 1909 + Microsoft 365 Apps
   - Windows Server 2019 Datacenter
   - Várias sessões do Windows 10 Enterprise, versão 2004
   - Windows 10 Enterprise multi-sessão, versão 2004 + Microsoft 365 Apps
2. Selecione **'Rever e criar' ( Revisão e Criação).**

## <a name="error-name_already_registered"></a>Erro: NAME_ALREADY_REGISTERED

O nome do seu VM já foi registado e é provavelmente um duplicado.

Para resolver este problema:
1. Siga os passos na [secção Remover o anfitrião da sessão da](#step-2-remove-the-session-host-from-the-host-pool) secção de piscina anfitriã.
2. [Criar outro VM](expand-existing-host-pool.md#add-virtual-machines-with-the-azure-portal). Certifique-se de escolher um nome único para este VM.
3. Vá ao [portal Azure](https://portal.azure.com) e abra a página **geral** para a piscina anfitriã onde o seu VM estava. 
4. Abra o separador **Session Hosts** e verifique se todos os anfitriões da sessão estão na piscina de anfitriões.
5. Aguarde 5-10 minutos para que o estado do anfitrião da sessão diga **disponível**.

   > [!div class="mx-imgBorder"]
   > ![Screenshot do anfitrião da sessão disponível](media/hostpool-portal.png)

## <a name="your-issue-isnt-listed-here-or-wasnt-resolved"></a>O seu problema não está listado aqui ou não foi resolvido.

Se não conseguir encontrar o seu problema neste artigo ou as instruções não o ajudarem, recomendamos que desinstale, reinstale e volte a registar o Windows Virtual Desktop Agent. As instruções desta secção mostram como voltar a registar o seu VM no serviço de ambiente de trabalho virtual do Windows, desinstalando todos os componentes do agente, carregador de arranque e pilha, retirando o anfitrião da sessão da piscina anfitriã, gerando uma nova chave de registo para o VM e reinstalando o agente e o carregador de arranque. Se um ou mais dos seguintes cenários se aplicarem a si, siga estas instruções:
- O seu VM está preso na **atualização** ou **indisponível**
- O seu ouvinte da pilha não está a funcionar e está a funcionar no Windows 10 1809, 1903 ou 1909
- Está a receber um erro **de EXPIRED_REGISTRATION_TOKEN.**
- Não estás a ver os teus VMs aparecerem na lista de anfitriões da sessão.
- Não se vê o **Carregador de Agente de Ambiente remoto** na janela dos serviços
- Não se vê o componente **RdAgentBootLoader** no Gestor de Tarefas
- Está a receber um **Corretor de Ligação que não conseguiu validar o** erro de definições em VMs de imagem personalizados
- As instruções deste artigo não resolveram o seu problema.

### <a name="step-1-uninstall-all-agent-boot-loader-and-stack-component-programs"></a>Passo 1: Desinstale todos os programas de componentes de agente, carregador de arranque e pilha

Antes de reinstalar o agente, o carregador de arranque e a pilha, tem de desinstalar quaisquer programas de componentes existentes a partir do seu VM. Para desinstalar todos os programas de agente, carregador de arranque e de empilhar componentes:
1. Inscreva-se no seu VM como administrador. 
2. Vá a Programas e  >  **Funcionalidades de Programas** de Controlo do Painel  >  .
3. Remova os seguintes programas:
   - Carregador de botas de agente de ambiente de trabalho remoto
   - Agente de infraestrutura de serviços de secretária remoto
   - Agente remoto da infraestrutura de serviços de desktop de Genebra
   - Pilha de rede SxS de serviços de desktop remoto
   
>[!NOTE]
>Pode ver vários casos destes programas. Certifique-se de removê-los todos.

   > [!div class="mx-imgBorder"]
   > ![Screenshot de programas de desinstalação](media/uninstall-program.png)

### <a name="step-2-remove-the-session-host-from-the-host-pool"></a>Passo 2: Retire o anfitrião da sessão da piscina anfitriã

Quando retirar o anfitrião da sessão da piscina anfitriã, o anfitrião da sessão já não está registado na piscina anfitriã. Isto funciona como um reset para o registo do anfitrião da sessão. Para remover o anfitrião da sessão da piscina anfitriã:
1. Aceda à página **geral** para a piscina de anfitrião em que se encontra o seu VM, no [portal Azure.](https://portal.azure.com) 
2. Vá ao separador **Session Hosts** para ver a lista de todos os anfitriões da sessão naquela piscina anfitriã.
3. Veja a lista de anfitriões da sessão e selecione o VM que pretende remover.
4. Selecione **Remover**.  

   > [!div class="mx-imgBorder"]
   > ![Screenshot de remover VM da piscina anfitriã](media/remove-sh.png)

### <a name="step-3-generate-a-new-registration-key-for-the-vm"></a>Passo 3: Gerar uma nova chave de registo para o VM

Tem de gerar uma nova chave de registo que seja usada para voltar a registar o seu VM na piscina de anfitrião e no serviço. Para gerar uma nova chave de registo para o VM:
1. Abra o [portal Azure](https://portal.azure.com) e vá à página **'Visão Geral'** para o pool de anfitriões do VM que pretende editar.
2. Selecione **a tecla de registo**.

   > [!div class="mx-imgBorder"]
   > ![Screenshot da chave de registo no portal](media/reg-key.png)

3. Abra o separador **chave de inscrição** e selecione **Gere a nova tecla.**
4. Introduza a data de validade e, em seguida, selecione **Ok**.  

>[!NOTE]
>A data de validade não pode ser inferior a uma hora e não mais de 27 dias a partir da sua data e hora de geração. Recomendamos vivamente que desfase a data de validade para o máximo de 27 dias.

5. Copie a chave recém-gerada para a sua área de transferência. Vai precisar desta chave mais tarde.

### <a name="step-4-reinstall-the-agent-and-boot-loader"></a>Passo 4: Reinstalar o agente e o carregador de arranque

Ao reinstalar a versão mais atualizada do agente e do carregador de arranque, a pilha lado a lado e o agente de monitorização de Genebra também são automaticamente instalados. Para reinstalar o agente e o carregador de arranque:
1. Inscreva-se no seu VM como administrador e utilize a versão correta do instalador do agente para a sua implementação, dependendo da versão do Windows que o seu VM está a executar. Se tiver um VM Do Windows 10, siga as instruções em [registar máquinas virtuais](create-host-pools-powershell.md#register-the-virtual-machines-to-the-windows-virtual-desktop-host-pool) para descarregar o **Windows Virtual Desktop Agent** e o Windows Virtual Desktop **Agent Bootloader**. Se tiver um VM Windows 7, siga os passos 13-14 em [máquinas virtuais de registo](deploy-windows-7-virtual-machine.md#configure-a-windows-7-virtual-machine) para descarregar o **Windows Virtual Desktop Agent** e o Windows Virtual Desktop **Agent Manager**.

   > [!div class="mx-imgBorder"]
   > ![Screenshot da página de descarregamento de agente e bootloader](media/download-agent.png)

2. Clique com o botão direito nos instaladores do carregador de botas e do agente que descarregou.
3. Selecione **Propriedades**.
4. Selecione **Desbloqueio**.
5. Selecione **OK**.
6. Execute o instalador do agente.
7. Quando o instalador lhe pedir o sinal de registo, cole a chave de registo da sua área de transferência. 

   > [!div class="mx-imgBorder"]
   > ![Screenshot do token de registo colado](media/pasted-agent-token.png)

8. Executar o instalador do carregador de arranque.
9. Reinicie o seu VM. 
10. Vá ao [portal Azure](https://portal.azure.com) e abra a página **Geral** para a piscina de anfitrião a que pertence o seu VM.
11. Vá ao separador **Session Hosts** para ver a lista de todos os anfitriões da sessão naquela piscina anfitriã.
12. Deverá agora ver o anfitrião da sessão registado na piscina anfitriã com o estado **disponível.** 

   > [!div class="mx-imgBorder"]
   > ![Screenshot do anfitrião da sessão disponível](media/hostpool-portal.png)

## <a name="next-steps"></a>Passos seguintes

Se o problema continuar, crie um caso de apoio e inclua informações detalhadas sobre o problema que está a ter e quaisquer ações que tenha tomado para tentar resolvê-lo. A lista que se segue inclui outros recursos que pode utilizar para resolver problemas na sua implementação virtual do Windows Desktop.

- Para obter uma visão geral sobre a resolução de problemas do Windows Virtual Desktop e as faixas de escalada, consulte [a visão geral da resolução de problemas, o feedback e o suporte](troubleshoot-set-up-overview.md).
- Para resolver problemas enquanto cria uma piscina de anfitriões num ambiente de ambiente de trabalho virtual do Windows, consulte [ambiente e a criação de piscinas de anfitriões.](troubleshoot-set-up-issues.md)
- Para resolver problemas enquanto configura uma máquina virtual (VM) no Windows Virtual Desktop, consulte a [configuração da máquina virtual do anfitrião da Sessão](troubleshoot-vm-configuration.md).
- Para resolver problemas com as ligações do cliente virtual do Windows Desktop, consulte [as ligações do serviço de desktop virtual do Windows](troubleshoot-service-connection.md).
- Para resolver problemas com clientes de ambiente de trabalho remoto, consulte [Troubleshoot o cliente Remote Desktop](troubleshoot-client.md).
- Para resolver problemas ao utilizar o PowerShell com o Windows Virtual Desktop, consulte [o Windows Virtual Desktop PowerShell](troubleshoot-powershell.md).
- Para saber mais sobre o serviço, consulte o [ambiente de ambiente de trabalho virtual do Windows.](environment-setup.md)
- Para passar por um tutorial de resolução de [problemas, consulte Tutorial: Implementações de modelos do Gestor de Recursos de Resolução de Problemas](../azure-resource-manager/templates/template-tutorial-troubleshoot.md).
- Para conhecer as ações de auditoria, consulte [as operações de Auditoria com o Gestor de Recursos.](../azure-resource-manager/management/view-activity-logs.md)
- Para obter ações para determinar os erros durante a implementação, consulte [as operações de implantação](../azure-resource-manager/templates/deployment-history.md)da visualização .
