---
title: Pré-requisitos para Azure AD Connect cloud sync em Azure AD
description: Este artigo descreve os pré-requisitos e requisitos de hardware necessários para sincronização na nuvem.
services: active-directory
author: billmath
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.topic: how-to
ms.date: 03/17/2021
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: 0277d4ce263610576178e3844a0665ab6506fbfa
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/30/2021
ms.locfileid: "104579166"
---
# <a name="prerequisites-for-azure-ad-connect-cloud-sync"></a>Pré-requisitos para sincronização de nuvem AZure AD Connect
Este artigo fornece orientações sobre como escolher e usar o Azure Ative Directory (Azure AD) Conecte a sincronização de nuvem como solução de identidade.

## <a name="cloud-provisioning-agent-requirements"></a>Requisitos do agente de provisionamento em nuvem
Precisa do seguinte para utilizar a sincronização de nuvem AD Connect Azure:

- Credenciais de Administrador de Domínio ou Administrador Empresarial para criar o Azure AD Connect Cloud Sync gMSA (conta de serviço gerido do grupo) para executar o serviço de agente. 
- Um administrador de identidade híbrido conta para o seu inquilino AZure AD que não é um utilizador convidado.
- Um servidor no local para o agente de provisionamento com o Windows 2016 ou mais tarde.  Este servidor deve ser um servidor de nível 0 baseado no [modelo de nível administrativo ative directory](/windows-server/identity/securing-privileged-access/securing-privileged-access-reference-material).
- Configurações de firewall no local.

## <a name="group-managed-service-accounts"></a>Contas de Serviço Geridas de Grupo
Um grupo Managed Service Account é uma conta de domínio gerida que fornece gestão automática de passwords, gestão de nome principal de serviço simplificado (SPN), a capacidade de delegar a gestão a outros administradores, e também estende esta funcionalidade a vários servidores.  Azure AD Connect Cloud Sync suporta e utiliza um gMSA para executar o agente.  Serão solicitados credenciais administrativas durante a configuração, de forma a criar esta conta.  A conta aparecerá como (domínio\provAgentgMSA$).  Para obter mais informações sobre um gMSA, consulte [contas de serviço geridas pelo grupo](/windows-server/security/group-managed-service-accounts/group-managed-service-accounts-overview) 

### <a name="prerequisites-for-gmsa"></a>Pré-requisitos para a GMSA:
1.  O esquema de Ative Directory na floresta do domínio gMSA precisa de ser atualizado para o Windows Server 2016.
2.  [Módulos PowerShell RSAT](/windows-server/remote/remote-server-administration-tools) num controlador de domínio
3.  Pelo menos um controlador de domínio no domínio deve estar a executar o Windows Server 2016.
4.  Um servidor de união de domínios onde o agente está a ser instalado precisa de ser o Windows Server 2016 ou mais tarde.

### <a name="custom-gmsa-account"></a>Conta gMSA personalizada
Se estiver a criar uma conta gMSA personalizada, tem de garantir que a conta tem as seguintes permissões.

|Tipo |Name |Access |Aplica-se A| 
|-----|-----|-----|-----|
|Permitir |conta gMSA |Ler todas as propriedades |Objetos de dispositivo descendente| 
|Permitir |conta gMSA|Ler todas as propriedades |Objetos inetOrgperson descendentes| 
|Permitir |conta gMSA |Ler todas as propriedades |Objetos de computador descendentes| 
|Permitir |conta gMSA |Ler todas as propriedades |Objetos de Segurança Estrangeira Descendentes| 
|Permitir |conta gMSA |Controlo total |Objetos de grupo descendentes| 
|Permitir |conta gMSA |Ler todas as propriedades |Objetos de utilizador descendentes| 
|Permitir |conta gMSA |Ler todas as propriedades |Objetos de contacto descendentes| 
|Permitir |conta gMSA |Criar/eliminar objetos do utilizador|Este objeto e todos os objetos descendentes| 

Para etapas sobre como atualizar um agente existente para utilizar uma conta gMSA consulte [contas de serviço geridas pelo grupo](how-to-install.md#group-managed-service-accounts).

### <a name="in-the-azure-active-directory-admin-center"></a>No centro de administração Azure Ative Directory

1. Crie uma conta de administrador de identidade híbrida apenas em nuvem no seu inquilino AD Azure. Desta forma, pode gerir a configuração do seu inquilino se os seus serviços no local falharem ou ficarem indisponíveis. Saiba como [adicionar uma conta de administrador de identidade híbrida apenas](../fundamentals/add-users-azure-active-directory.md)na nuvem . Terminar este passo é fundamental para garantir que não fique trancado fora do seu inquilino.
1. Adicione um ou mais [nomes de domínio personalizados](../fundamentals/add-custom-domain.md) ao seu inquilino AZure AD. Os seus utilizadores podem iniciar sôms com um destes nomes de domínio.

### <a name="in-your-directory-in-active-directory"></a>No seu diretório em Diretório Ativo

Executar a [ferramenta IdFix](/office365/enterprise/prepare-directory-attributes-for-synch-with-idfix) para preparar os atributos do diretório para sincronização.

### <a name="in-your-on-premises-environment"></a>No seu ambiente no local

1. Identifique um servidor anfitrião ligado a domínio que executa o Windows Server 2016 ou superior com um mínimo de 4-GB de RAM e .NET 4.7.1+ tempo de execução.

2. A política de execução PowerShell no servidor local deve ser definida para Undefined ou RemoteSigned.

3. Se houver uma firewall entre os seus servidores e Azure AD, configuure os seguintes itens:
    - Certifique-se de que os agentes podem fazer pedidos *de saída* à Azure AD nas seguintes portas:

      | Número da porta | Como é utilizado |
      | --- | --- |
      | **80** | Descarrega as listas de revogação de certificados (CRLs) ao mesmo tempo que valida o certificado TLS/SSL.  |
      | **443** | Lida com todas as comunicações de saída com o serviço. |
      | **8080** (opcional) | Os agentes reportam o seu estado a cada 10 minutos sobre o porto 8080, se a porta 443 não estiver disponível. Este estado é apresentado no portal AD Azure. |

    - Se a firewall impuser regras de acordo com os utilizadores de origem, abra estas portas para o tráfego dos serviços Windows que são executados com um serviço de rede.
    - Se a sua firewall ou proxy permitir que especifique sufixos seguros, adicione ligações a \* .msappproxy.net e \* .servicebus.windows.net. Caso contrário, permita o acesso aos intervalos IP do [datacenter Azure](https://www.microsoft.com/download/details.aspx?id=41653), que são atualizados semanalmente.
    - Os agentes precisam de acesso a login.windows.net e login.microsoftonline.com para o registo inicial. Abra a firewall para os URLs também.
    - Para validação de certificados, desbloqueie os seguintes URLs: mscrl.microsoft.com:80, crl.microsoft.com:80, ocsp.msocsp.com:80 e www \. microsoft.com:80. Estes URLs são utilizados na validação de certificados com outros produtos Microsoft, pelo que talvez já tenha estes URLs desbloqueados.

    >[!NOTE]
    > A instalação do agente de provisionamento em nuvem no Windows Server Core não é suportada.

### <a name="additional-requirements"></a>Requisitos adicionais

- [Quadro Microsoft .NET 4.7.1](https://www.microsoft.com/download/details.aspx?id=56116) 

#### <a name="tls-requirements"></a>Requisitos TLS

> [!NOTE]
> Transport Layer Security (TLS) é um protocolo que prevê comunicações seguras. A alteração das definições de TLS afeta toda a floresta. Para obter mais informações, consulte [Update para ativar os TLS 1.1 e TLS 1.2 como protocolos seguros predefinidos no WinHTTP no Windows](https://support.microsoft.com/help/3140245/update-to-enable-tls-1-1-and-tls-1-2-as-default-secure-protocols-in-wi).

O servidor Windows que acolhe o agente de provisionamento de nuvem Azure AD Connect deve ter o TLS 1.2 ativado antes de o instalar.

Para ativar o TLS 1.2, siga estes passos.

1. Definir as seguintes chaves de registo:

    ```
    [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2]
    [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2\Client] "DisabledByDefault"=dword:00000000 "Enabled"=dword:00000001
    [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2\Server] "DisabledByDefault"=dword:00000000 "Enabled"=dword:00000001
    [HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\.NETFramework\v4.0.30319] "SchUseStrongCrypto"=dword:00000001
    ```

1. Reinicie o servidor.

## <a name="known-limitations"></a>Limitações conhecidas

São conhecidas as seguintes limitações:

### <a name="delta-synchronization"></a>Sincronização Delta

- A filtragem de âmbito de grupo para a sincronização delta não suporta mais de 1500 membros.
- Quando eliminar um grupo que é usado como parte de um filtro de deteção de grupo, os utilizadores que são membros do grupo, não sejam eliminados. 
- Quando mudar o nome da UO ou do grupo que está no âmbito, a Delta Sync não removerá os utilizadores.

### <a name="provisioning-logs"></a>Registos de Aprovisionamento
- Os registos de provisionamento não diferenciam claramente entre as operações de criação e atualização.  Pode ver uma operação de criação para uma atualização e uma operação de atualização para uma criação.

### <a name="group-re-naming-or-ou-re-naming"></a>Renomeação do grupo ou rebatição de U
- Se mudar o nome de um grupo ou ou em AD que está no âmbito de uma determinada configuração, o trabalho de sincronização de nuvem não será capaz de reconhecer a mudança de nome em AD. O trabalho não vai para a quarentena e permanecerá saudável.


## <a name="next-steps"></a>Passos seguintes 

- [O que é o aprovisionamento?](what-is-provisioning.md)
- [O que é a sincronização de nuvem AZure AD Connect?](what-is-cloud-sync.md)