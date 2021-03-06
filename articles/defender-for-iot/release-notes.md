---
title: Novidades no Azure Defender para ioT
description: Este artigo permite-lhe saber quais as novidades no mais recente lançamento do Defender for IoT.
ms.topic: overview
ms.date: 04/19/2021
ms.openlocfilehash: da5358ccf0f69ca2ba8f5722b75889b6b7c92c07
ms.sourcegitcommit: 6686a3d8d8b7c8a582d6c40b60232a33798067be
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/20/2021
ms.locfileid: "107752611"
---
# <a name="whats-new-in-azure-defender-for-iot"></a>O que há de novo no Azure Defender para ioT?

Este artigo lista novas funcionalidades e melhorias de funcionalidades para Defender para IoT.

As funcionalidades notadas estão em PREVIEW. Os [Termos Complementares de Pré-visualização do Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) incluem termos legais adicionais aplicáveis às funcionalidades do Azure que estão em versão beta, pré-visualização ou ainda não lançadas em disponibilidade geral.

## <a name="april-2021"></a>abril de 2021

### <a name="work-with-automatic-threat-intelligence-updates-public-preview"></a>Trabalhar com atualizações automáticas de inteligência de ameaça (Visualização pública)

Os novos pacotes de inteligência de ameaças podem agora ser automaticamente empurrados para sensores ligados à nuvem, uma vez que são lançados pelo Microsoft Defender para IoT. Isto além de descarregar pacotes de inteligência de ameaças e depois enviá-los para sensores.

Trabalhar com atualizações automáticas ajuda a reduzir os esforços operacionais e a garantir uma maior segurança. Ativar a atualização automática, a bordo do sensor ligado à nuvem no portal Defender para IoT, com o **alternador de Atualizações automáticas de Inteligência** de Ameaças ligados.

Se quiser ter uma abordagem mais conservadora para atualizar os seus dados de inteligência de ameaça, pode empurrar manualmente pacotes do portal Azure Defender para ioT para sensores ligados à nuvem apenas quando sentir que é necessário.
Isto dá-lhe a capacidade de controlar quando um pacote é instalado, sem a necessidade de descarregar e depois carregá-lo para os seus sensores. Empurre manualmente atualizações para os sensores da página Defender para **Sites e Sensores** IoT.

Também pode rever as seguintes informações sobre pacotes de inteligência de ameaças:

- Versão do pacote instalada
- Modo de atualização de inteligência de ameaça 
- Estado de atualização de inteligência de ameaça

### <a name="view-cloud-connected-sensor-information-public-preview"></a>Ver informações de sensores ligados à nuvem (Visualização pública)

Consulte informações operacionais importantes sobre sensores ligados à nuvem na página **'Sites e Sensores'.**

- A versão sensor instalada
- O estado de ligação do sensor à nuvem.
- A última vez que o sensor foi detetado a ligar-se à nuvem.

### <a name="alert-api-enhancements"></a>Melhorias da API de alerta

Novos campos estão disponíveis para utilizadores que trabalhem com APIs de alerta.

**Consola de gestão no local**

- Endereço de origem e destino
- Passos de remediação
- O nome do sensor definido pelo utilizador
- O nome da zona associada ao sensor 
- O nome do site associado ao sensor

**Sensor**

- Endereço de origem e destino
- Passos de remediação

A versão 2 da API é necessária ao trabalhar com os novos campos.

### <a name="features-delivered-as-generally-available-ga"></a>Funcionalidades entregues como Geralmente Disponíveis (GA)

As seguintes funcionalidades estavam anteriormente disponíveis para visualização pública, e estão agora geralmente disponíveis (GA)características:

- Sensor - regras de alerta personalizadas melhoradas
- Consola de gestão no local - alertas de exportação
- Adicionar segunda interface de rede à consola de gestão de instalações
- Construtor de dispositivos - novo micro agente

## <a name="march-2021"></a>março de 2021

### <a name="sensor---enhanced-custom-alert-rules-public-preview"></a>Sensor - regras de alerta personalizadas melhoradas (Visualização pública)

Agora pode criar regras de alerta personalizadas com base no dia, foi detetada a atividade da rede de dias e de período de tempo.  Trabalhar com as condições de regra do dia e do tempo é útil, por exemplo, nos casos em que a gravidade do alerta é derivada no momento em que o evento de alerta ocorre. Por exemplo, crie uma regra personalizada que desencadeie um alerta de alta gravidade quando a atividade da rede é detetada num fim de semana ou à noite.

Esta funcionalidade encontra-se disponível no sensor com o lançamento da versão 10.2.

### <a name="on-premises-management-console---export-alerts-public-preview"></a>Consola de gestão no local - alertas de exportação (Visualização pública)

As informações de alerta podem agora ser exportadas para um ficheiro .csv a partir da consola de gestão no local. Pode exportar informações de todos os alertas detetados ou exportar informações com base na vista filtrada.

Esta funcionalidade encontra-se disponível na consola de gestão no local com o lançamento da versão 10.2.

### <a name="add-second-network-interface-to-on-premises-management-console-public-preview"></a>Adicionar segunda interface de rede à consola de gestão on-in (Visualização pública)

Pode agora aumentar a segurança da sua implementação adicionando uma segunda interface de rede à sua consola de gestão no local. Esta funcionalidade permite que a sua gestão no local tenha os seus sensores conectados numa rede segura, permitindo ao utilizador aceder à consola de gestão no local através de uma segunda interface de rede separada.

Esta funcionalidade encontra-se disponível na consola de gestão no local com o lançamento da versão 10.2.

### <a name="add-second-network-interface-to-on-premises-management-console-public-preview"></a>Adicionar segunda interface de rede à consola de gestão on-in (visualização pública)

Pode agora aumentar a segurança da sua implementação adicionando uma segunda interface de rede à sua consola de gestão no local. Esta funcionalidade permite que a sua gestão no local tenha os seus sensores conectados numa rede segura, permitindo ao utilizador aceder à consola de gestão no local através de uma segunda interface de rede separada.

Esta funcionalidade encontra-se disponível na consola de gestão no local com o lançamento da versão 10.2.
### <a name="device-builder---new-micro-agent-public-preview"></a>Construtor de dispositivos - novo micro-agente (visualização pública)

Um novo módulo de construtor de dispositivos está disponível. O módulo, referido como micro-agente, permite:

- **Integração com o Azure IoT Hub e Azure Defender para IoT** - construa uma segurança final mais forte diretamente nos seus dispositivos IoT, integrando-a com a opção de monitorização fornecida tanto pelo Azure IoT Hub como pelo Azure Defender para IoT.
- **Opções de implementação flexíveis com suporte para sistemas operativos IoT padrão** - podem ser implementadas quer como um pacote binário ou como código fonte modificável, com suporte para sistemas operativos IoT padrão como Linux e Azure RTOS.
- **Requisitos mínimos de recursos sem dependências de kernel de SO** - pequena pegada, baixo consumo de CPU, e sem dependências de núcleos de OS.
- **Gestão da postura de** segurança – monitorize proativamente a postura de segurança dos seus dispositivos IoT.
- **Deteção contínua, em tempo real de ameaças de IoT/OT** - detetar ameaças como botnets, tentativas de força bruta, mineradores de cripto e atividades suspeitas de rede

A documentação precedida defender-ioT-micro-agente será transferida para a *solução baseada no Agente para construtores de dispositivos>* pasta Classic.

Este conjunto de funcionalidades está disponível com o lançamento em nuvem de pré-visualização do público atual.

## <a name="january-2021"></a>Janeiro de 2021

- [Segurança](#security)
- [Inclusão](#onboarding)
- [Usabilidade](#usability)
- [Outras atualizações](#other-updates)
### <a name="security"></a>Segurança

Foram feitas melhorias na recuperação de certificados e passwords para esta versão.

#### <a name="certificates"></a>Certificados
  
Esta versão permite-lhe:

- Faça o upload dos certificados SSL diretamente para os sensores e consolas de gestão no local.
- Execute a validação entre a consola de gestão no local e os sensores conectados, e entre uma consola de gestão e uma consola de gestão de Alta Disponibilidade. A validação baseia-se em datas de validade, autenticidade de CA de raiz e listas de revogação de certificados.  Se a validação falhar, a sessão não continuará.

Para upgrades:

- Não há alteração no certificado SSL ou na funcionalidade de validação durante a atualização.
- Após a atualização, os utilizadores administrativos de consolas de gestão de sensores e no local podem substituir certificados SSL ou ativar a validação de certificadoS SSL a partir da janela 'Definições do Sistema' SSL.  

Para instalações frescas:

- Durante o início de sessão, os utilizadores são obrigados a utilizar um Certificado SSL (recomendado) ou um certificado auto-assinado gerado localmente (não recomendado)
- A validação do certificado é ligada por defeito para instalações frescas.

#### <a name="password-recovery"></a>Recuperação de senha
  
Sensor e consola de gestão no local Os utilizadores administrativos podem agora recuperar senhas do Azure Defender para o portal IoT. Anteriormente, a recuperação da palavra-passe exigia a intervenção da equipa de apoio.

### <a name="onboarding"></a>Inclusão

#### <a name="on-premises-management-console---committed-devices"></a>Consola de gestão no local - dispositivos comprometidos

Após o primeiro sôs-in na consola de gestão no local, os utilizadores passam a ser obrigados a carregar um ficheiro de ativação. O ficheiro contém o número agregado de dispositivos a serem monitorizados na rede organizacional. Este número é referido como o número de dispositivos comprometidos.
Os dispositivos comprometidos são definidos durante o processo de embarque no portal Azure Defender para IoT, onde o ficheiro de ativação é gerado.
Os utilizadores e a atualização da primeira vez são obrigados a carregar o ficheiro de ativação pela primeira vez.
Após a ativação inicial, o número de dispositivos detetados na rede pode exceder o número de dispositivos comprometidos. Este evento pode acontecer, por exemplo, se ligar mais sensores à consola de gestão. Se houver uma discrepância entre o número de dispositivos detetados e o número de dispositivos comprometidos, aparece um aviso na consola de gestão. Se este evento ocorrer, deverá carregar um novo ficheiro de ativação.

#### <a name="pricing-page-options"></a>Opções de página de preços

A página de preços permite-lhe embarcar novas subscrições do Azure Defender para IoT e definir dispositivos comprometidos na sua rede.  
Além disso, a página de Preços permite-lhe agora gerir as subscrições existentes associadas a um compromisso de sensor e dispositivo de atualização.

#### <a name="view-and-manage-onboarded-sensors"></a>Ver e gerir sensores a bordo

Uma nova página do portal site e sensores permite-lhe:

- Adicione informações descritivas sobre o sensor. Por exemplo, uma zona associada ao sensor ou etiquetas de texto livre.
- Ver e filtrar informações do sensor. Por exemplo, ver detalhes sobre sensores que estão ligados à nuvem ou geridos localmente ou ver informações sobre sensores numa zona específica.  

### <a name="usability"></a>Usabilidade

#### <a name="azure-sentinel-new-connector-page"></a>Página de conector Azure Sentinel

A página de conector de dados Azure Defender para IoT em Azure Sentinel foi redesenhada. O conector de dados baseia-se agora em subscrições e não em IoT Hubs; permitindo aos clientes gerir melhor a sua ligação de configuração ao Azure Sentinel.

#### <a name="azure-portal-permission-updates"></a>Atualizações de permissão do portal Azure  

O suporte do Leitor de Segurança e do Administrador de Segurança foi adicionado.

### <a name="other-updates"></a>Outras atualizações

#### <a name="access-group---zone-permissions"></a>Grupo de acesso - permissões de zona
  
As regras do Grupo de Acesso às consolas de gestão no local não incluirão a opção de conceder acesso a uma zona específica. Não há qualquer alteração na definição de regras que utilizem sites, regiões e unidades de negócio.   Após a atualização, os Grupos de Acesso que continham regras que permitissem o acesso a zonas específicas serão modificados para permitir o acesso ao seu site principal, incluindo todas as suas zonas.

#### <a name="terminology-changes"></a>Alterações de terminologia

O termo ativo foi renomeado dispositivo na consola de gestão de sensores e no local, relatórios e outras interfaces de solução.
Nos alertas de consola de gestão de sensores e no local, o termo Gerir este Evento foi nomeado Passos de Remediação.

## <a name="next-steps"></a>Passos seguintes

[Começar com o Defender para o IoT](getting-started.md)
