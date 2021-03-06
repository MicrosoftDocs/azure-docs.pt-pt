---
title: Linha de base de segurança Azure para tecido de serviço
description: A linha de base de segurança do Tecido de Serviço fornece orientações processuais e recursos para a implementação das recomendações de segurança especificadas no Benchmark de Segurança Azure.
author: msmbaldwin
ms.service: service-fabric
ms.topic: conceptual
ms.date: 04/08/2021
ms.author: mbaldwin
ms.custom: subject-security-benchmark
ms.openlocfilehash: ee294cff85bb71b5c2a238fcf985fc870ed32618
ms.sourcegitcommit: c6a2d9a44a5a2c13abddab932d16c295a7207d6a
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/09/2021
ms.locfileid: "107285467"
---
# <a name="azure-security-baseline-for-service-fabric"></a>Linha de base de segurança Azure para tecido de serviço

Esta linha de base de segurança aplica orientações da [versão Azure Security Benchmark1.0](../security/benchmarks/overview-v1.md) para o Service Fabric. A Referência de Segurança do Azure disponibiliza recomendações para proteger as suas soluções cloud no Azure. O conteúdo é agrupado pelos **controlos de segurança definidos** pelo Azure Security Benchmark e pela orientação conexa aplicável ao Service Fabric.

> [!NOTE]
> **Foram excluídos os controlos** não aplicáveis ao Service Fabric, ou para os quais a responsabilidade é da Microsoft. Para ver como o Tecido de Serviço mapeia completamente para o Benchmark de Segurança Azure, consulte o **[ficheiro de mapeamento de base de segurança de segurança do Service Fabric](https://github.com/MicrosoftDocs/SecurityBenchmarks/raw/master/Azure%20Offer%20Security%20Baselines/1.1/service-fabric-security-baseline-v1.1.xlsx)**.

## <a name="network-security"></a>Segurança de Rede

*Para obter mais informações, veja [Referência de Segurança do Azure: Segurança de Rede](../security/benchmarks/security-control-network-security.md).*

### <a name="11-protect-azure-resources-within-virtual-networks"></a>1.1: Proteger os recursos do Azure nas redes virtuais

**Orientação**: Certifique-se de que todas as implementações da sub-rede virtual têm um grupo de segurança de rede aplicado com controlos de acesso à rede específicos das portas e fontes fidedignas da sua aplicação. 

- [Implementar o Azure Firewall através de um modelo](../firewall/deploy-template.md)

- [Criar redes de perímetro utilizando grupos de segurança da rede Azure (NSGs)](https://docs.microsoft.com/azure/security/fundamentals/service-fabric-best-practices#use-network-isolation-and-security-with-azure-service-fabric)

- [Como integrar o seu cluster de tecido de serviço Azure com uma rede virtual existente](service-fabric-patterns-networking.md)

**Responsabilidade**: Cliente

**Monitorização do Centro de Segurança Azure**: Nenhum

### <a name="12-monitor-and-log-the-configuration-and-traffic-of-virtual-networks-subnets-and-nics"></a>1.2: Monitorizar e registar a configuração e o tráfego de redes virtuais, sub-redes e NICs

**Orientação**: Utilize o Centro de Segurança e remedia as recomendações de proteção da rede para a rede virtual, sub-rede e grupo de segurança da rede que está a ser utilizado para proteger o seu cluster de tecidos de serviço Azure. Ativar os registos de fluxo do grupo de segurança da rede e enviar registos numa Conta de Armazenamento Azure para auditoria de tráfego. Também pode enviar registos de fluxo de grupos de segurança de rede para um espaço de trabalho Azure Log Analytics e utilizar o Azure Traffic Analytics para fornecer informações sobre o fluxo de tráfego na sua nuvem Azure. Algumas vantagens da Azure Traffic Analytics são a capacidade de visualizar a atividade da rede e identificar pontos quentes, identificar ameaças de segurança, compreender padrões de fluxo de tráfego e identificar erros de configuração da rede.

- [Como ativar registos de fluxo NSG](../network-watcher/network-watcher-nsg-flow-logging-portal.md)

- [Como Ativar e utilizar a Azure Traffic Analytics](../network-watcher/traffic-analytics.md)

- [Compreender a Segurança da Rede fornecida pelo Azure Security Center](../security-center/security-center-network-recommendations.md)

**Responsabilidade**: Cliente

**Monitorização do Centro de Segurança Azure**: Nenhum

### <a name="13-protect-critical-web-applications"></a>1.3: Proteger aplicações web críticas

**Orientação**: Forneça um porta de entrada frontal para fornecer um único ponto de entrada para utilizadores, dispositivos ou outras aplicações. A Azure API Management integra-se diretamente com o Service Fabric, permitindo-lhe garantir o acesso a serviços back-end, prevenir ataques DOS utilizando estrangulamentos e verificar chaves API, fichas JWT, certificados e outras credenciais.

Considere implementar o Azure Web Application Firewall (WAF) em frente a aplicações web críticas para inspeção adicional do tráfego de entrada. Ativar a Definição de Diagnóstico para WAF e ingerir registos numa conta de armazenamento, no Centro de Eventos ou no Espaço de Trabalho do Log Analytics.

- [Service Fabric with Azure API Management overview](service-fabric-api-management-overview.md)(Service Fabric com descrição geral da Gestão de API do Azure)

- [Integrar a Gestão de API numa VNET interna com o Gateway de Aplicação](../api-management/api-management-howto-integrate-internal-vnet-appgateway.md)

- [Como implantar a Azure WAF](../web-application-firewall/ag/create-waf-policy-ag.md)

**Responsabilidade**: Cliente

**Monitorização do Centro de Segurança Azure**: Nenhum

### <a name="14-deny-communications-with-known-malicious-ip-addresses"></a>1.4: Negar comunicações com endereços IP maliciosos conhecidos

**Orientação**: Para proteções contra ataques DDoS, permita a proteção padrão Azure DDoS na rede virtual onde o seu cluster de tecido de serviço Azure está implantado. Use o Azure Security Center integrado em inteligência de ameaça para negar comunicações com endereços IP de Internet maliciosos ou não utilizados.

- [Como configurar a proteção DDoS](../ddos-protection/manage-ddos-protection.md)

- [Compreender a Azure Security Center Integrada Desespionagem de Ameaças](../security-center/azure-defender.md)

**Responsabilidade**: Cliente

**Monitorização do Centro de Segurança Azure**: Nenhum

### <a name="15-record-network-packets"></a>1.5: Pacotes de rede de registos

**Orientação**: Ative os registos de fluxo do grupo de segurança da rede para os grupos de segurança da rede ligados à sub-rede que está a ser utilizada para proteger o seu cluster de tecido de serviço. Grave os registos de fluxo NSG numa Conta de Armazenamento Azure para gerar registos de fluxo. Se necessário para investigar atividades anómalas, ative a captura de pacotes do Azure Network Watcher.

- [Como ativar registos de fluxo NSG](../network-watcher/network-watcher-nsg-flow-logging-portal.md)

- [Como ativar o Observador de Redes](../network-watcher/network-watcher-create.md)

- [Use análises de tráfego para visualizar registos de fluxo NSG](../network-watcher/traffic-analytics.md)

**Responsabilidade**: Cliente

**Monitorização do Centro de Segurança Azure**: Nenhum

### <a name="16-deploy-network-based-intrusion-detectionintrusion-prevention-systems-idsips"></a>1.6: Implementar sistemas de deteção/prevenção de intrusões baseados em rede (IDS/IPS)

**Orientação**: Selecione uma oferta do Azure Marketplace que suporta a funcionalidade IDS/IPS com capacidades de inspeção de carga útil.  Se a deteção e/ou prevenção de intrusões baseada na inspeção de carga útil não for um requisito, a Azure Firewall com Threat Intelligence pode ser usada. A filtragem baseada em inteligência Azure Firewall Threat pode alertar e negar tráfego de e para endereços ip maliciosos conhecidos e domínios. Os domínios e endereços IP são obtidos a partir do feed das Informações sobre Ameaças da Microsoft. 

Implemente a solução de firewall à sua escolha em cada um dos limites de rede da sua organização para detetar e/ou negar tráfego malicioso. 

- [Azure Marketplace](https://azuremarketplace.microsoft.com/marketplace/?term=Firewall) 

- [Como implantar a Firewall do Azure](../firewall/tutorial-firewall-deploy-portal.md) 

- [Como configurar alertas com a Azure Firewall](../firewall/threat-intel.md)

**Responsabilidade**: Cliente

**Monitorização do Centro de Segurança Azure**: Nenhum

### <a name="17-manage-traffic-to-web-applications"></a>1.7: Gerir o tráfego para aplicações web

**Orientação**: Implementar gateway de aplicações Azure para aplicações web com HTTPS/SSL habilitado para certificados fidedignos. 

- [Como implementar o Gateway de Aplicações](../application-gateway/quick-create-portal.md)

- [Como configurar o Gateway de aplicações para utilizar HTTPS](../application-gateway/create-ssl-portal.md)

- [Compreenda o equilíbrio de carga da camada 7 com os gateways de aplicações web Azure](../application-gateway/overview.md)

**Responsabilidade**: Cliente

**Monitorização do Centro de Segurança Azure**: Nenhum

### <a name="18-minimize-complexity-and-administrative-overhead-of-network-security-rules"></a>1.8: Minimizar a complexidade e a sobrecarga administrativa das regras de segurança da rede

**Orientação**: Utilize tags de serviço de rede virtuais para definir controlos de acesso à rede em grupos de segurança de rede (NSG) que estejam ligados à sub-rede onde o cluster Azure Service Fabric é implantado. Ao criar regras de segurança, pode utilizar etiquetas de serviço em vez de endereços IP específicos. Ao especificar o nome da etiqueta de serviço (por exemplo, ApiManagement) no campo de origem ou destino adequado de uma regra, pode permitir ou negar o tráfego para o serviço correspondente. A Microsoft gere os prefixos de endereços englobados pela etiqueta de serviço e atualiza automaticamente a etiqueta de serviço à medida que os endereços mudam.

- [Tags de serviço de rede virtual](../virtual-network/service-tags-overview.md)

- [Networking de tecido de serviço as melhores práticas](service-fabric-best-practices-networking.md)

**Responsabilidade**: Cliente

**Monitorização do Centro de Segurança Azure**: Nenhum

### <a name="19-maintain-standard-security-configurations-for-network-devices"></a>1.9: Manter configurações de segurança padrão para dispositivos de rede

**Orientação**: Defina e implemente configurações de segurança padrão para recursos de rede relacionados com o seu cluster de tecido de serviço Azure. Utilize pseudónimos da Azure Policy nos espaços de nome "Microsoft.ServiceFabric" e "Microsoft.Network" para criar políticas personalizadas para auditar ou impor a configuração de rede do seu cluster de tecido de serviço Azure.

Também pode usar plantas Azure para simplificar as implementações de Azure em larga escala através de artefactos de ambiente chave de embalagem, tais como modelos de Gestor de Recursos Azure, controlos e políticas do Azure RBAC, e políticas, numa única definição de planta. Aplique facilmente o projeto em novas subscrições e ambientes, e afinar o controlo e a gestão através da versão.

- [Como ver pseudónimos disponíveis da Política Azure](https://docs.microsoft.com/powershell/module/az.resources/get-azpolicyalias?view=azps-4.8.0&amp;preserve-view=true)

- [Como configurar e gerir o Azure Policy](../governance/policy/tutorials/create-and-manage.md)

- [Como criar uma Planta Azure](../governance/blueprints/create-blueprint-portal.md)

**Responsabilidade**: Cliente

**Monitorização do Centro de Segurança Azure**: Nenhum

### <a name="110-document-traffic-configuration-rules"></a>1.10: Regras de configuração do tráfego documental

**Orientação**: Utilize etiquetas para grupos de segurança de rede e outros recursos relacionados com a segurança da rede e o fluxo de tráfego associados ao seu cluster de Tecidos de Serviço. Para as regras individuais dos grupos de segurança da rede, utilize o campo "Descrição" para especificar a necessidade, duração e assim por diante do negócio, para quaisquer regras que permitam o tráfego de/para uma rede.

Utilize qualquer uma das definições de Política Azure incorporadas relacionadas com a marcação, tais como "Exigir etiqueta e seu valor" para garantir que todos os recursos são criados com Tags e para notificá-lo dos recursos existentes não marcados.

Pode utilizar a interface de linha de comando Azure PowerShell ou Azure (CLI) para procurar ou executar ações em recursos baseados nas suas Tags.

- [Como criar e usar Tags](../azure-resource-manager/management/tag-resources.md)

- [Como criar uma rede virtual](../virtual-network/quick-create-portal.md)

- [Como criar um NSG com um Config de Segurança](../virtual-network/tutorial-filter-network-traffic.md)

**Responsabilidade**: Cliente

**Monitorização do Centro de Segurança Azure**: Nenhum

### <a name="111-use-automated-tools-to-monitor-network-resource-configurations-and-detect-changes"></a>1.11: Utilize ferramentas automatizadas para monitorizar as configurações de recursos de rede e detetar alterações

**Orientação**: Utilize o Registo de Atividades Azure para monitorizar as configurações de recursos de rede e detetar alterações para os recursos de rede relacionados com as suas implementações do Tecido de Serviço Azure. Crie alertas dentro do Azure Monitor que irão desencadear quando ocorrerem alterações nos recursos críticos da rede.

- [Como visualizar e recuperar eventos de Registo de Atividades Azure](/azure/azure-monitor/platform/activity-log#view-the-activity-log)

- [Como criar alertas no Azure Monitor](/azure/azure-monitor/platform/alerts-activity-log)

**Responsabilidade**: Cliente

**Monitorização do Centro de Segurança Azure**: Nenhum

## <a name="logging-and-monitoring"></a>Início de sessão e Monitorização

*Para obter mais informações, consulte o [Benchmark de Segurança Azure: Registo e monitorização](../security/benchmarks/security-control-logging-monitoring.md).*

### <a name="22-configure-central-security-log-management"></a>2.2: Configurar a gestão central dos registos de segurança

**Orientação**: Pode embarcar o seu cluster de Tecido de Serviço Azure para o Azure Monitor para agregar dados de segurança gerados pelo cluster. Veja exemplo problemas de diagnóstico e soluções com o Service Fabric.

- [Configurar a integração de registos do Azure Monitor com o Tecido de Serviço](service-fabric-diagnostics-oms-setup.md)

- [Configurar registos do Monitor Azure para monitorização de contentores em Tecido de Serviço Azure](service-fabric-tutorial-monitoring-wincontainers.md)

- [Diagnóstico de cenários comuns de tecido de serviço](service-fabric-diagnostics-common-scenarios.md)

**Responsabilidade**: Cliente

**Monitorização do Centro de Segurança Azure**: Nenhum

### <a name="23-enable-audit-logging-for-azure-resources"></a>2.3: Permitir a exploração de auditorias para recursos Azure

**Orientação**: Ative o Monitor Azure para o cluster de tecido de serviço, direcione-o para um espaço de trabalho Log Analytics. Isto registará informações relevantes do cluster e métricas de SO para todos os nós de cluster de tecido de serviço Azure.

- [Configurar a integração de registos do Azure Monitor com o Tecido de Serviço](service-fabric-diagnostics-oms-setup.md)

- [Configurar registos do Monitor Azure para monitorização de contentores em Tecido de Serviço Azure](service-fabric-tutorial-monitoring-wincontainers.md)

- [Como colocar o agente Log Analytics nos seus nós](service-fabric-diagnostics-oms-agent.md)

- [Registar pesquisas de registos de analíticas](/azure/azure-monitor/log-query/log-query-overview)

**Responsabilidade**: Cliente

**Monitorização do Centro de Segurança Azure**: Nenhum

### <a name="24-collect-security-logs-from-operating-systems"></a>2.4: Recolher registos de segurança dos sistemas operativos

**Orientação**: A bordo do cluster de tecido de serviço Azure para o Monitor Azure. Certifique-se de que o espaço de trabalho Log Analytics utilizado tem o período de retenção de registo definido de acordo com os regulamentos de conformidade da sua organização.

- [Configurar a integração de registos do Azure Monitor com o Tecido de Serviço](service-fabric-diagnostics-oms-setup.md)

- [Configurar registos do Monitor Azure para monitorização de contentores em Tecido de Serviço Azure](service-fabric-tutorial-monitoring-wincontainers.md)

- [Como colocar o agente Log Analytics nos seus nós](service-fabric-diagnostics-oms-agent.md) 

- [Como configurar o período de retenção do espaço de trabalho do Log Analytics](/azure/azure-monitor/platform/manage-cost-storage)

**Responsabilidade**: Cliente

**Monitorização do Centro de Segurança Azure**: Nenhum

### <a name="25-configure-security-log-storage-retention"></a>2.5: Configurar a retenção de armazenamento de registos de segurança

**Orientação**: A bordo do cluster de tecido de serviço para o Monitor Azure. Certifique-se de que o espaço de trabalho Log Analytics utilizado tem o período de retenção de registo definido de acordo com os regulamentos de conformidade da sua organização.

- [Configurar a integração de registos do Azure Monitor com o Tecido de Serviço](service-fabric-diagnostics-oms-setup.md)

- [Configurar registos do Monitor Azure para monitorização de contentores em Tecido de Serviço Azure](service-fabric-tutorial-monitoring-wincontainers.md)

- [Como colocar o agente Log Analytics nos seus nós](service-fabric-diagnostics-oms-agent.md)

- [Como configurar o período de retenção do espaço de trabalho do Log Analytics](/azure/azure-monitor/platform/manage-cost-storage)

**Responsabilidade**: Cliente

**Monitorização do Centro de Segurança Azure**: Nenhum

### <a name="26-monitor-and-review-logs"></a>2.6: Registos de monitorização e revisão

**Orientação**: Utilize consultas de espaço de trabalho Azure Log Analytics para consultar os registos de tecido de serviço Azure.

- [Registar pesquisas de registos de analíticas](/azure/azure-monitor/log-query/log-query-overview)

**Responsabilidade**: Cliente

**Monitorização do Centro de Segurança Azure**: Nenhum

### <a name="27-enable-alerts-for-anomalous-activities"></a>2.7: Permitir alertas para atividades anómalas

**Orientação**: Utilize o espaço de trabalho Azure Log Analytics para monitorizar e alertar sobre atividades anómalas em registos de segurança e eventos relacionados com o seu cluster de tecido de serviço Azure.

- [Como gerir alertas no Centro de Segurança Azure](../security-center/security-center-managing-and-responding-alerts.md)

- [Como alertar nos dados de registo de registo de registos de registos](/azure/azure-monitor/learn/tutorial-response)

**Responsabilidade**: Cliente

**Monitorização do Centro de Segurança Azure**: Nenhum

### <a name="28-centralize-anti-malware-logging"></a>2.8: Centralizar a sessão anti-malware

**Orientação**: Por predefinição, o Windows Defender é instalado no Windows Server 2016. Consulte a documentação antimaleware para obter regras de configuração se não estiver a utilizar o Windows Defender. O Windows Defender não é suportado no Linux.

- [Para mais detalhes, consulte o antivírus do Windows Defender no Windows Server 2016](/windows/security/threat-protection/windows-defender-antivirus/windows-defender-antivirus-on-windows-server-2016)

**Responsabilidade**: Cliente

**Monitorização do Centro de Segurança Azure**: Nenhum

### <a name="29-enable-dns-query-logging"></a>2.9: Ativar a sessão de consulta de DNS

**Orientação**: Implementar uma solução de terceiros para a registo de DNS.

**Responsabilidade**: Cliente

**Monitorização do Centro de Segurança Azure**: Nenhum

### <a name="210-enable-command-line-audit-logging"></a>2.10: Permitir a exploração de auditoria de linha de comando

**Orientação**: Configurar manualmente o registo da consola numa base por nó.

**Responsabilidade**: Cliente

**Monitorização do Centro de Segurança Azure**: Nenhum

## <a name="identity-and-access-control"></a>Identidade e Controlo de Acesso

*Para mais informações, consulte o [Benchmark de Segurança Azure: Identidade e Controlo de Acesso.](../security/benchmarks/security-control-identity-access-control.md)*

### <a name="31-maintain-an-inventory-of-administrative-accounts"></a>3.1: Manter um inventário das contas administrativas

**Orientação**: Mantenha o registo da conta administrativa local que é criada durante o a provisionamento do cluster do cluster de tecidos de serviço Azure, bem como quaisquer outras contas que crie. Além disso, se a integração do Azure Ative Directory (Azure AD), a Azure AD tem funções incorporadas que devem ser explicitamente atribuídas e, portanto, são questionáveis. Utilize o módulo Azure AD PowerShell para realizar consultas de adhoc para descobrir contas que são membros de grupos administrativos.

Além disso, pode utilizar recomendações de Gestão de Identidade e Acesso do Centro de Segurança Azure.

- [Como obter um papel de diretório em Azure AD com PowerShell](https://docs.microsoft.com/powershell/module/azuread/get-azureaddirectoryrole?view=azureadps-2.0&amp;preserve-view=true)

- [Como obter membros de um papel de diretório em Azure AD com PowerShell](https://docs.microsoft.com/powershell/module/azuread/get-azureaddirectoryrolemember?view=azureadps-2.0&amp;preserve-view=true)

- [Como monitorizar a identidade e o acesso com o Azure Security Center](../security-center/security-center-identity-access.md)

**Responsabilidade**: Cliente

**Monitorização do Centro de Segurança Azure**: Nenhum

### <a name="32-change-default-passwords-where-applicable"></a>3.2: Alterar palavras-passe padrão quando aplicável

**Orientação**: Ao providenciar um cluster, o Azure exige que crie novas palavras-passe para o portal web. Não existem senhas padrão para alterar, no entanto pode especificar diferentes palavras-passe para acesso ao portal.

- [Criar no portal do Azure](service-fabric-cluster-creation-via-portal.md)

**Responsabilidade**: Cliente

**Monitorização do Centro de Segurança Azure**: Nenhum

### <a name="33-use-dedicated-administrative-accounts"></a>3.3: Utilizar contas administrativas dedicadas

**Orientação**: Integrar autenticação para tecido de serviço com diretório ativo Azure (Azure AD). Criar políticas e procedimentos em torno da utilização de contas administrativas dedicadas.

Além disso, pode utilizar recomendações de Gestão de Identidade e Acesso do Centro de Segurança.

- [Configurar a autenticação do cliente Azure AD](https://docs.microsoft.com/azure/service-fabric/service-fabric-tutorial-create-vnet-and-windows-cluster#set-up-azure-active-directory-client-authentication)

- [Como monitorizar a identidade e o acesso com o Azure Security Center](../security-center/security-center-identity-access.md)

**Responsabilidade**: Cliente

**Monitorização do Centro de Segurança Azure**: Nenhum

### <a name="34-use-single-sign-on-sso-with-azure-active-directory"></a>3.4: Utilize um único sinal de sso com o Azure Ative Directory

**Orientação**: Sempre que possível, utilize o Azure Ative Directory (Azure AD) SSO em vez de configurar credenciais individuais autónomas por serviço. Use recomendações de Gestão de Identidade e Acesso do Centro de Segurança. 

- [Compreender SSO com Azure AD](../active-directory/manage-apps/what-is-single-sign-on.md)

**Responsabilidade**: Cliente

**Monitorização do Centro de Segurança Azure**: Nenhum

### <a name="35-use-multi-factor-authentication-for-all-azure-active-directory-based-access"></a>3.5: Utilize a autenticação multi-factor para todos os acessos baseados no Azure Ative Directory

**Orientação**: Permitir a autenticação multifactora Azure Ative (Azure AD) e seguir as recomendações de Gestão de Identidade e Acesso do Centro de Segurança.

- [Como permitir a autenticação multifactor em Azure](../active-directory/authentication/howto-mfa-getstarted.md)

- [Como monitorizar a identidade e o acesso dentro do Centro de Segurança Azure](../security-center/security-center-identity-access.md)

**Responsabilidade**: Cliente

**Monitorização do Centro de Segurança Azure**: Nenhum

### <a name="36-use-dedicated-machines-privileged-access-workstations-for-all-administrative-tasks"></a>3.6: Utilizar máquinas dedicadas (Estações de acesso privilegiadas) para todas as tarefas administrativas

**Orientação**: Utilize a Estação de Trabalho de Acesso Privilegiado (PAW) com autenticação multifactor configurada para iniciar sessão e configurar os seus clusters de Tecido de Serviço e recursos conexos.

- [Saiba mais sobre estações de trabalho de acesso privilegiado](https://4sysops.com/archives/understand-the-microsoft-privileged-access-workstation-paw-security-model/)

- [Como permitir a autenticação multifactor em Azure](../active-directory/authentication/howto-mfa-getstarted.md)

**Responsabilidade**: Cliente

**Monitorização do Centro de Segurança Azure**: Nenhum

### <a name="37-log-and-alert-on-suspicious-activities-from-administrative-accounts"></a>3.7: Registar e alertar sobre atividades suspeitas a partir de contas administrativas

**Orientação**: Utilize o Azure Ative Directory (Azure AD) Gestão de Identidade Privilegiada (PIM) para a geração de registos e alertas quando ocorrem atividades suspeitas ou inseguras no ambiente. Além disso, utilize deteções de risco Azure AD para visualizar alertas e relatórios sobre o comportamento do utilizador de risco.

- [Como implementar gestão de identidade privilegiada (PIM)](../active-directory/privileged-identity-management/pim-deployment-plan.md)

- [Compreender deteções de risco Azure AD](../active-directory/identity-protection/overview-identity-protection.md)

**Responsabilidade**: Cliente

**Monitorização do Centro de Segurança Azure**: Nenhum

### <a name="38-manage-azure-resources-from-only-approved-locations"></a>3.8: Gerir os recursos do Azure a partir de locais aprovados apenas

**Orientação**: Utilize locais nomeados de acesso condicional para permitir o acesso a partir de agrupamentos lógicos específicos de intervalos de endereços IP ou países/regiões. 

- [Como configurar localizações nomeadas em Azure](../active-directory/reports-monitoring/quickstart-configure-named-locations.md)

**Responsabilidade**: Cliente

**Monitorização do Centro de Segurança Azure**: Nenhum

### <a name="39-use-azure-active-directory"></a>3.9: Utilizar o Diretório Ativo Azure

**Orientação**: Utilize o Azure Ative Directory (Azure AD) como o sistema central de autenticação e autorização para garantir o acesso aos pontos finais de gestão dos clusters de Tecidos de Serviço. A Azure AD protege os dados utilizando uma encriptação forte para dados em repouso e em trânsito. A Azure AD também sai, hashes e armazena seguramente as credenciais dos utilizadores.

- [Como criar e configurar instâncias do Azure AD](../active-directory/fundamentals/active-directory-access-create-new-tenant.md)

- [Configuração Azure AD para autenticação do cliente do Service Fabric](service-fabric-cluster-creation-setup-aad.md)

**Responsabilidade**: Cliente

**Monitorização do Centro de Segurança Azure**: O [Azure Security Benchmark](/azure/governance/policy/samples/azure-security-benchmark) é a iniciativa política padrão do Centro de Segurança e é a base para as [recomendações do Centro de Segurança.](/azure/security-center/security-center-recommendations) As definições da Política Azure relacionadas com este controlo são ativadas automaticamente pelo Centro de Segurança. Os alertas relacionados com este controlo podem requerer um plano [Azure Defender](/azure/security-center/azure-defender) para os serviços relacionados.

**Definições incorporadas da Azure Policy - Microsoft.ServiceFabric**:

[!INCLUDE [Resource Policy for Microsoft.ServiceFabric 3.9](../../includes/policy/standards/asb/rp-controls/microsoft.servicefabric-3-9.md)]

### <a name="310-regularly-review-and-reconcile-user-access"></a>3.10: Reveja e reconciliar regularmente o acesso dos utilizadores

**Orientação**: Utilize a autenticação do Azure Ative Directory (Azure AD) com o seu cluster de Tecido de Serviço. A Azure AD fornece registos para ajudar a descobrir contas velhas. Além disso, utilize a Azure Identity Access Reviews para gerir eficientemente os membros do grupo, o acesso a aplicações empresariais e atribuições de funções. O acesso do utilizador pode ser revisto regularmente para garantir que apenas os Utilizadores certos tenham acesso continuado.

- [Como utilizar comentários sobre acesso à identidade do Azure](../active-directory/governance/access-reviews-overview.md)

**Responsabilidade**: Cliente

**Monitorização do Centro de Segurança Azure**: Nenhum

### <a name="311-alert-on-account-login-behavior-deviation"></a>3.11: Alerta sobre desvio de comportamento de login de conta

**Orientação**: Utilize registos de inscrição e auditoria do Azure Ative (Azure AD) para monitorizar as tentativas de acesso a contas desativadas; estes registos podem ser integrados em qualquer ferramenta siem/monitorização de terceiros.

Pode agilizar este processo criando Definições de Diagnóstico para contas de utilizadores AZure, enviando os registos de auditoria e registos de login para um espaço de trabalho Azure Log Analytics. Configure os alertas desejados dentro do espaço de trabalho Azure Log Analytics.

- [Como integrar os Registos de Atividades do Azure no Azure Monitor](../active-directory/reports-monitoring/howto-integrate-activity-logs-with-log-analytics.md)

**Responsabilidade**: Cliente

**Monitorização do Centro de Segurança Azure**: Nenhum

### <a name="312-alert-on-account-sign-in-behavior-deviation"></a>3.12: Alerta sobre desvio de comportamento de inscrição na conta

**Orientação**: Utilize funcionalidades de Proteção de Risco e Identidade Azure Ative (Azure AD) para configurar respostas automatizadas para detetar ações suspeitas relacionadas com identidades dos utilizadores. Você também pode ingerir dados em Azure Sentinel para mais investigação.

- [Como ver os inícios de sessão de risco do Azure AD](../active-directory/identity-protection/overview-identity-protection.md)

- [Como configurar e permitir políticas de risco de proteção de identidade](../active-directory/identity-protection/howto-identity-protection-configure-risk-policies.md)

- [Como embarcar Azure Sentinel](../sentinel/quickstart-onboard.md)

**Responsabilidade**: Cliente

**Monitorização do Centro de Segurança Azure**: Nenhum

## <a name="data-protection"></a>Proteção de Dados

*Para obter mais informações, veja [Referência de Segurança do Azure: Proteção de dados](../security/benchmarks/security-control-data-protection.md).*

### <a name="41-maintain-an-inventory-of-sensitive-information"></a>4.1: Manter um inventário de informação sensível

**Orientação**: Utilize etiquetas em recursos relacionados com as implementações do cluster do seu Tecido de Serviço para ajudar a rastrear os recursos da Azure que armazenam ou processam informações sensíveis.

- [Como criar e usar tags](../azure-resource-manager/management/tag-resources.md)

**Responsabilidade**: Cliente

**Monitorização do Centro de Segurança Azure**: Nenhum

### <a name="42-isolate-systems-storing-or-processing-sensitive-information"></a>4.2: Isolar sistemas de armazenamento ou tratamento de informações sensíveis

**Orientação**: Implementar subscrições separadas e/ou grupos de gestão para desenvolvimento, teste e produção. Os recursos devem ser separados por Rede Virtual ou sub-rede, marcados adequadamente e protegidos por um grupo de segurança de rede ou firewall Azure. Os recursos que armazenam ou processam dados sensíveis devem ser suficientemente isolados. Para máquinas virtuais armazenar ou processar dados sensíveis, implementar políticas e procedimentos para desligá-los quando não estiverem a ser utilizados. 

- [Como criar subscrições adicionais do Azure](../cost-management-billing/manage/create-subscription.md)

- [Como criar Grupos de Gestão](../governance/management-groups/create-management-group-portal.md)

- [Como criar e usar Tags](../azure-resource-manager/management/tag-resources.md)

- [Como criar uma Rede Virtual](../virtual-network/quick-create-portal.md)

- [Como criar um NSG com um Config de Segurança](../virtual-network/tutorial-filter-network-traffic.md)

- [Como implantar a Firewall do Azure](../firewall/tutorial-firewall-deploy-portal.md)

- [Como configurar alerta ou alerta e negar com a Azure Firewall](../firewall/threat-intel.md)

**Responsabilidade**: Cliente

**Monitorização do Centro de Segurança Azure**: Nenhum

### <a name="43-monitor-and-block-unauthorized-transfer-of-sensitive-information"></a>4.3: Monitor e bloquear transferência não autorizada de informações sensíveis

**Orientação**: Implemente uma ferramenta automatizada em perímetros de rede que monitorize para transferência não autorizada de informações sensíveis e bloqueie tais transferências enquanto alerta os profissionais de segurança da informação. 

Para a plataforma subjacente, gerida pela Microsoft, a Microsoft trata todos os conteúdos dos clientes como sensíveis e faz grandes esforços para se proteger contra a perda e exposição de dados dos clientes. Para garantir que os dados dos clientes dentro do Azure permanecem seguros, a Microsoft implementou e mantém um conjunto de controlos e capacidades robustos de proteção de dados. 

- [Compreender a proteção dos dados dos clientes no Azure](../security/fundamentals/protection-customer-data.md)

**Responsabilidade**: Partilhada

**Monitorização do Centro de Segurança Azure**: Nenhum

### <a name="44-encrypt-all-sensitive-information-in-transit"></a>4.4: Criptografar todas as informações sensíveis em trânsito

**Orientação**: Criptografe todas as informações sensíveis em trânsito. Certifique-se de que qualquer cliente que se conecte aos seus recursos Azure seja capaz de negociar TLS 1.2 ou superior. 

Para autenticação mútua de cliente a nó de Serviço, utilize um certificado X.509 para identidade do servidor e encriptação TLS de comunicação http. Qualquer número de certificados adicionais pode ser instalado num cluster para fins de segurança da aplicação, incluindo encriptação e desencriptação dos valores de configuração da aplicação e dados em todos os nós durante a replicação.
Siga as recomendações do Centro de Segurança para encriptação em repouso e encriptação em trânsito, quando aplicável. 

- [Compreender a encriptação em trânsito com Azure](https://docs.microsoft.com/azure/security/fundamentals/encryption-overview#encryption-of-data-in-transit)

- [Cenários de segurança do cluster de tecido de serviço](service-fabric-cluster-security.md)

- [Guia de resolução de problemas do tecido de serviço para configuração de TLS](https://github.com/Azure/Service-Fabric-Troubleshooting-Guides/blob/master/Security/TLS%20Configuration.md)

**Responsabilidade**: Partilhada

**Monitorização do Centro de Segurança Azure**: Nenhum

### <a name="45-use-an-active-discovery-tool-to-identify-sensitive-data"></a>4.5: Utilize uma ferramenta de descoberta ativa para identificar dados sensíveis

**Orientação**: As características de identificação, classificação e prevenção de perdas ainda não estão disponíveis para o Azure Storage ou para calcular recursos. Implementar solução de terceiros, se necessário para efeitos de conformidade.

Para a plataforma subjacente, gerida pela Microsoft, a Microsoft trata todos os conteúdos dos clientes como sensíveis e faz grandes esforços para se proteger contra a perda e exposição de dados dos clientes. Para garantir que os dados dos clientes dentro do Azure permanecem seguros, a Microsoft implementou e mantém um conjunto de controlos e capacidades robustos de proteção de dados.

- [Compreender a proteção dos dados dos clientes no Azure](../security/fundamentals/protection-customer-data.md)

**Responsabilidade**: Partilhada

**Monitorização do Centro de Segurança Azure**: Nenhum

### <a name="47-use-host-based-data-loss-prevention-to-enforce-access-control"></a>4.7: Utilizar a prevenção da perda de dados baseada no hospedeiro para impor o controlo do acesso

**Orientação**: Para os clusters de tecidos de serviço que armazenam ou processam informações sensíveis, marque o cluster e os recursos conexos como sensíveis através de tags. As funcionalidades de identificação, classificação e prevenção de perdas ainda não estão disponíveis para o Azure Storage ou para calcular recursos. Implementar solução de terceiros, se necessário para efeitos de conformidade.

Para a plataforma subjacente, gerida pela Microsoft, a Microsoft trata todos os conteúdos dos clientes como sensíveis e faz grandes esforços para se proteger contra a perda e exposição de dados dos clientes. Para garantir que os dados dos clientes dentro do Azure permanecem seguros, a Microsoft implementou e mantém um conjunto de controlos e capacidades robustos de proteção de dados.

- [Compreender a proteção dos dados dos clientes no Azure](../security/fundamentals/protection-customer-data.md)

**Responsabilidade**: Partilhada

**Monitorização do Centro de Segurança Azure**: Nenhum

### <a name="48-encrypt-sensitive-information-at-rest"></a>4.8: Criptografe informação sensível em repouso

**Orientação**: Utilize a encriptação em repouso em todos os recursos Azure. A Microsoft recomenda que o Azure gere as suas chaves de encriptação, no entanto existe a opção de gerir as suas próprias chaves em alguns casos. 

- [Compreender a encriptação de dados inativos no Azure](../security/fundamentals/encryption-atrest.md)  

- [Como configurar chaves de encriptação geridas pelo cliente](../storage/common/customer-managed-keys-configure-key-vault.md)

- [Ativar encriptação de disco para os nóns de cluster de tecido de serviço Azure no Windows](service-fabric-enable-azure-disk-encryption-windows.md)

- [Ativar encriptação de disco para os nódos de cluster de tecido de serviço Azure em Linux](service-fabric-enable-azure-disk-encryption-linux.md)

**Responsabilidade**: Cliente

**Monitorização do Centro de Segurança Azure**: O [Azure Security Benchmark](/azure/governance/policy/samples/azure-security-benchmark) é a iniciativa política padrão do Centro de Segurança e é a base para as [recomendações do Centro de Segurança.](/azure/security-center/security-center-recommendations) As definições da Política Azure relacionadas com este controlo são ativadas automaticamente pelo Centro de Segurança. Os alertas relacionados com este controlo podem requerer um plano [Azure Defender](/azure/security-center/azure-defender) para os serviços relacionados.

**Definições incorporadas da Azure Policy - Microsoft.ServiceFabric**:

[!INCLUDE [Resource Policy for Microsoft.ServiceFabric 4.8](../../includes/policy/standards/asb/rp-controls/microsoft.servicefabric-4-8.md)]

### <a name="49-log-and-alert-on-changes-to-critical-azure-resources"></a>4.9: Registar e alertar sobre alterações aos recursos críticos do Azure

**Orientação**: Utilize o Monitor Azure com o Registo de Atividades Azure para criar alertas para quando ocorrerem alterações para recursos críticos do Azure. 

- [Como criar alertas para eventos de Registo de Atividades Azure](/azure/azure-monitor/platform/alerts-activity-log)

**Responsabilidade**: Cliente

**Monitorização do Centro de Segurança Azure**: Nenhum

## <a name="vulnerability-management"></a>Gestão de Vulnerabilidades

*Para mais informações, consulte o [Azure Security Benchmark: Vulnerability Management](../security/benchmarks/security-control-vulnerability-management.md).*

### <a name="51-run-automated-vulnerability-scanning-tools"></a>5.1: Executar ferramentas automatizadas de digitalização de vulnerabilidades

**Orientação**: Gere regularmente os serviços de Análise de Falhas de Tecido de Serviço e Caos para simular falhas em todo o cluster para avaliar a robustez e fiabilidade dos seus serviços.

Siga as recomendações do Security Center sobre a realização de avaliações de vulnerabilidade nas suas máquinas virtuais Azure e imagens de contentores. 

Utilize uma solução de terceiros para realizar avaliações de vulnerabilidade em dispositivos de rede e aplicações web. Ao realizar exames remotos, não utilize uma única conta administrativa perpétua. Considere implementar a metodologia de provisionamento do JIT para a conta de digitalização. As credenciais para a conta de digitalização devem ser protegidas, monitorizadas e utilizadas apenas para a verificação de vulnerabilidades. 

- [Introdução ao Serviço de Análise de Falhas de Tecido de Serviço](service-fabric-testability-overview.md)

- [Induzir o caos controlado nos clusters de tecido de serviço](service-fabric-controlled-chaos.md)

- [Como implementar recomendações de avaliação de vulnerabilidade do Azure Security Center](../security-center/deploy-vulnerability-assessment-vm.md)

**Responsabilidade**: Cliente

**Monitorização do Centro de Segurança Azure**: Nenhum

### <a name="52-deploy-automated-operating-system-patch-management-solution"></a>5.2: Implementar solução automatizada de gestão de correção do sistema operativo

**Orientação**: Ative atualizações automáticas de imagem de SO nos conjuntos de escala de máquina virtual do seu cluster de Tecido de Serviço. 

Em alternativa, para testar primeiro as correções de SO antes de ir à produção, utilize o gatilho manual para atualizações de imagem de SO do seu conjunto de escala. Note que a opção de gatilho manual não fornece reversão incorporada. Monitorize patches de SO utilizando a Gestão de Atualização da Azure Automation. 

- [Gestão de patch para os nosdes de cluster de tecido de serviço](https://docs.microsoft.com/azure/service-fabric/service-fabric-best-practices-infrastructure-as-code#azure-virtual-machine-operating-system-automatic-upgrade-configuration)

- [Upgrades automáticos de imagem de SO com conjuntos de escala de máquina virtual Azure](../virtual-machine-scale-sets/virtual-machine-scale-sets-automatic-upgrade.md)

- [Como aproximar os VMs com o modelo mais recente da escala](https://docs.microsoft.com/azure/virtual-machine-scale-sets/virtual-machine-scale-sets-upgrade-scale-set#how-to-bring-vms-up-to-date-with-the-latest-scale-set-model)

- [Visão geral da Azure Automation Update Management](/azure/automation/update-management/update-mgmt-overview)

**Responsabilidade**: Cliente

**Monitorização do Centro de Segurança Azure**: Nenhum

### <a name="53-deploy-automated-patch-management-solution-for-third-party-software-titles"></a>5.3: Implementar solução automatizada de gestão de patchs para títulos de software de terceiros

**Orientação**: Ative atualizações automáticas de imagem de SO nos conjuntos de escala de máquina virtual do seu cluster de tecido de serviço Azure. Patch Orchestration Application (POA) é uma solução alternativa que se destina a clusters de Tecido de Serviço hospedados fora de Azure. PoA pode ser usado com clusters Azure, com algumas despesas adicionais de hospedagem.

- [Gestão de patch para os nosdes de cluster de tecido de serviço](https://docs.microsoft.com/azure/service-fabric/service-fabric-best-practices-infrastructure-as-code#azure-virtual-machine-operating-system-automatic-upgrade-configuration)

- [Upgrades automáticos de imagem de SO com conjuntos de escala de máquina virtual Azure](../virtual-machine-scale-sets/virtual-machine-scale-sets-automatic-upgrade.md)

- [Como configurar o calendário de remendos de SISTEMA para clusters de tecido de serviço](service-fabric-patch-orchestration-application.md)

**Responsabilidade**: Cliente

**Monitorização do Centro de Segurança Azure**: Nenhum

### <a name="54-compare-back-to-back-vulnerability-scans"></a>5.4: Comparar digitalizações de vulnerabilidades consecutivas

**Orientação**: Exportar resultados de digitalização a intervalos consistentes e comparar os resultados para verificar se as vulnerabilidades foram remediadas. Ao utilizar recomendações de gestão de vulnerabilidades sugeridas pelo Security Center, pode entrar no portal da solução selecionada para visualizar dados históricos de digitalização.

**Responsabilidade**: Cliente

**Monitorização do Centro de Segurança Azure**: Nenhum

### <a name="55-use-a-risk-rating-process-to-prioritize-the-remediation-of-discovered-vulnerabilities"></a>5.5: Utilize um processo de classificação de risco para priorizar a reparação de vulnerabilidades descobertas

**Orientação**: Utilize um programa comum de pontuação de risco (por exemplo, sistema comum de pontuação de vulnerabilidade) ou as classificações de risco padrão fornecidas pela sua ferramenta de digitalização de terceiros.

**Responsabilidade**: Cliente

**Monitorização do Centro de Segurança Azure**: Nenhum

## <a name="inventory-and-asset-management"></a>Gestão de Recursos e Inventário

*Para mais informações, consulte o [Azure Security Benchmark: Inventário e Gestão de Ativos.](../security/benchmarks/security-control-inventory-asset-management.md)*

### <a name="61-use-automated-asset-discovery-solution"></a>6.1: Utilize uma solução automatizada de descoberta de ativos

**Orientação**: Utilize o Azure Resource Graph para consultar/descobrir todos os recursos (tais como computação, armazenamento, rede, portas e protocolos, etc.) dentro da sua(s) subscrição( s). Certifique-se de permissões (de leitura) adequadas no seu inquilino e enumera todas as subscrições do Azure, bem como os recursos dentro das suas subscrições.

Embora os recursos clássicos do Azure possam ser descobertos através do Gráfico de Recursos, é altamente recomendado criar e utilizar recursos do Azure Resource Manager.

- [Como criar consultas com gráfico de recursos Azure](../governance/resource-graph/first-query-portal.md)

- [Como ver as suas Subscrições Azure](https://docs.microsoft.com/powershell/module/az.accounts/get-azsubscription?view=azps-4.8.0&amp;preserve-view=true)

- [Compreender Azure RBAC](../role-based-access-control/overview.md)

**Responsabilidade**: Cliente

**Monitorização do Centro de Segurança Azure**: Nenhum

### <a name="62-maintain-asset-metadata"></a>6.2: Manter metadados de ativos

**Orientação**: Aplicar etiquetas aos recursos Azure, dando metadados para organizar logicamente numa taxonomia.

- [Como criar e usar tags](../azure-resource-manager/management/tag-resources.md)

**Responsabilidade**: Cliente

**Monitorização do Centro de Segurança Azure**: Nenhum

### <a name="63-delete-unauthorized-azure-resources"></a>6.3: Eliminar recursos azure não autorizados

**Orientação**: Utilize marcação, grupos de gestão e assinaturas separadas, se for caso disso, para organizar e rastrear ativos. Conciliar o inventário regularmente e garantir que os recursos não autorizados sejam eliminados da subscrição em tempo útil.

- [Como criar subscrições adicionais do Azure](../cost-management-billing/manage/create-subscription.md)

- [Como criar Grupos de Gestão](../governance/management-groups/create-management-group-portal.md)

- [Como criar e usar tags](../azure-resource-manager/management/tag-resources.md)

**Responsabilidade**: Cliente

**Monitorização do Centro de Segurança Azure**: Nenhum

### <a name="64-define-and-maintain-inventory-of-approved-azure-resources"></a>6.4: Definir e manter o inventário dos recursos aprovados da Azure

**Orientação**: Defina recursos Azure aprovados e software aprovado para recursos de computação.

**Responsabilidade**: Cliente

**Monitorização do Centro de Segurança Azure**: Nenhum

### <a name="65-monitor-for-unapproved-azure-resources"></a>6.5: Monitor para recursos Azure não aprovados

**Orientação**: Utilize a Política Azure para impor restrições ao tipo de recursos que podem ser criados na subscrição(s) de subscrição de clientes, utilizando as seguintes definições de política incorporada:

Tipos de recursos não permitidos

Tipos de recursos permitidos

Utilize o Gráfico de Recursos Azure para consultar/descobrir recursos dentro das suas subscrições. Certifique-se de que todos os recursos Azure presentes no ambiente são aprovados.

- [Como configurar e gerir o Azure Policy](../governance/policy/tutorials/create-and-manage.md)

- [Como criar consultas com Azure Graph](../governance/resource-graph/first-query-portal.md)

**Responsabilidade**: Cliente

**Monitorização do Centro de Segurança Azure**: Nenhum

### <a name="66-monitor-for-unapproved-software-applications-within-compute-resources"></a>6.6: Monitor para aplicações de software não aprovadas dentro dos recursos computacional

**Orientação**: Implementar uma solução de terceiros para monitorizar os nós de cluster para aplicações de software não aprovadas.

**Responsabilidade**: Cliente

**Monitorização do Centro de Segurança Azure**: Nenhum

### <a name="67-remove-unapproved-azure-resources-and-software-applications"></a>6.7: Remover recursos e aplicações de software não aprovados

**Orientação**: Utilize o Azure Resource Graph para consultar/descobrir todos os recursos (tais como computação, armazenamento, rede, portas e protocolos, etc.), incluindo clusters de Tecido de Serviço, dentro das suas subscrições. Remova quaisquer recursos não aprovados do Azure que descobrir. Para os nós de cluster de tecido de serviço, implemente uma solução de terceiros para remover ou alertar em software não aprovado.

- [Como criar consultas com Azure Graph](../governance/resource-graph/first-query-portal.md)

**Responsabilidade**: Cliente

**Monitorização do Centro de Segurança Azure**: Nenhum

### <a name="68-use-only-approved-applications"></a>6.8: Utilizar apenas aplicações aprovadas

**Orientação**: Para os nós de cluster de tecido de serviço, implemente uma solução de terceiros para impedir a execução de software não autorizado.

**Responsabilidade**: Cliente

**Monitorização do Centro de Segurança Azure**: Nenhum

### <a name="69-use-only-approved-azure-services"></a>6.9: Utilizar apenas serviços Azure aprovados

**Orientação**: Utilize a Política Azure para restringir os serviços que pode prestação no seu ambiente.

- [Como configurar e gerir o Azure Policy](../governance/policy/tutorials/create-and-manage.md)

- [Como negar um tipo específico de recurso com a Política Azure](https://docs.microsoft.com/azure/governance/policy/samples/built-in-policies#general)

**Responsabilidade**: Cliente

**Monitorização do Centro de Segurança Azure**: Nenhum

### <a name="610-maintain-an-inventory-of-approved-software-titles"></a>6.10: Manter um inventário de títulos de software aprovados

**Orientação**: Para os nós de cluster de tecido de serviço Azure, implemente uma solução de terceiros para evitar a execução de tipos de ficheiros não autorizados.

**Responsabilidade**: Cliente

**Monitorização do Centro de Segurança Azure**: Nenhum

### <a name="611-limit-users-ability-to-interact-with-azure-resource-manager"></a>6.11: Limitar a capacidade dos utilizadores de interagirem com o Gestor de Recursos Azure

**Orientação**: Utilize o Acesso Condicional Azure para limitar a capacidade dos utilizadores de interagirem com o Azure Resources Manager, configurando o "Acesso ao Bloco" para a app "Microsoft Azure Management". 

- [Como configurar o Acesso Condicional para bloquear o acesso ao Gestor de Recursos Azure](../role-based-access-control/conditional-access-azure-management.md)

**Responsabilidade**: Cliente

**Monitorização do Centro de Segurança Azure**: Nenhum

### <a name="612-limit-users-ability-to-execute-scripts-within-compute-resources"></a>6.12: Limitar a capacidade dos utilizadores de executar scripts dentro dos recursos computacional

**Orientação**: Utilize configurações específicas do sistema operativo ou recursos de terceiros para limitar a capacidade dos utilizadores de executar scripts dentro dos recursos de computação Azure.

- [Por exemplo, como controlar a execução de scripts PowerShell em Ambientes windows](https://docs.microsoft.com/powershell/module/microsoft.powershell.security/set-executionpolicy?view=powershell-7&amp;preserve-view=true)

**Responsabilidade**: Cliente

**Monitorização do Centro de Segurança Azure**: Nenhum

### <a name="613-physically-or-logically-segregate-high-risk-applications"></a>6.13: Segregar física ou logicamente aplicações de alto risco

**Orientação**: O software necessário para operações comerciais, mas que pode incorrer em maior risco para a organização, deve ser isolado dentro da sua própria máquina virtual e/ou rede virtual e suficientemente protegido com um Azure Firewall ou um Grupo de Segurança de Rede. 

- [Como criar uma rede virtual](../virtual-network/quick-create-portal.md) 

- [Como criar um NSG com um Config de Segurança](../virtual-network/tutorial-filter-network-traffic.md)

**Responsabilidade**: Cliente

**Monitorização do Centro de Segurança Azure**: Nenhum

## <a name="secure-configuration"></a>Configuração Segura

*Para obter mais informações, consulte o [Benchmark de Segurança Azure: Configuração Segura](../security/benchmarks/security-control-secure-configuration.md).*

### <a name="71-establish-secure-configurations-for-all-azure-resources"></a>7.1: Estabelecer configurações seguras para todos os recursos da Azure

**Orientação**: Utilize pseudónimos da Política Azure no espaço de nomes "Microsoft.ServiceFabric" para criar políticas personalizadas para auditar ou impor a configuração de rede do seu cluster de Tecido de Serviço.

- [Como ver pseudónimos disponíveis da Política Azure](https://docs.microsoft.com/powershell/module/az.resources/get-azpolicyalias?view=azps-4.8.0&amp;preserve-view=true)

- [Como configurar e gerir o Azure Policy](../governance/policy/tutorials/create-and-manage.md)

**Responsabilidade**: Cliente

**Monitorização do Centro de Segurança Azure**: Nenhum

### <a name="72-establish-secure-operating-system-configurations"></a>7.2: Estabelecer configurações seguras do sistema operativo

**Orientação**: As imagens do sistema operativo do sistema operativo de tecido de serviço são geridas e mantidas pela Microsoft. Cliente responsável pela implementação de configurações seguras para o sistema operativo dos seus nós de cluster.

**Responsabilidade**: Cliente

**Monitorização do Centro de Segurança Azure**: Nenhum

### <a name="73-maintain-secure-azure-resource-configurations"></a>7.3: Manter configurações seguras de recursos Azure

**Orientação**: Use a Política Azure [negar] e [implementar se não existir] efeitos para impor definições seguras para os seus clusters de tecido de serviço Azure e recursos relacionados.

- [Como configurar e gerir o Azure Policy](../governance/policy/tutorials/create-and-manage.md)

- [Compreender efeitos da política do Azure](../governance/policy/concepts/effects.md)

**Responsabilidade**: Cliente

**Monitorização do Centro de Segurança Azure**: Nenhum

### <a name="74-maintain-secure-operating-system-configurations"></a>7.4: Manter configurações seguras do sistema operativo

**Orientação**: Conjunto de tecido de serviço Sistema operativo Imagens geridas e mantidas pela Microsoft. O cliente é responsável pela implementação da configuração do estado de nível oss.

**Responsabilidade**: Partilhada

**Monitorização do Centro de Segurança Azure**: Nenhum

### <a name="75-securely-store-configuration-of-azure-resources"></a>7.5: Armazenar de forma segura a configuração dos recursos Azure

**Orientação**: Se utilizar definições personalizadas da Política Azure, utilize Azure DevOps ou Azure Repos para armazenar e gerir o seu código de forma segura.

- [Como armazenar código em Azure DevOps](https://docs.microsoft.com/azure/devops/repos/git/gitworkflow?view=azure-devops&amp;preserve-view=true)

- [Documentação de Azure Repos](https://docs.microsoft.com/azure/devops/repos/?view=azure-devops&amp;preserve-view=true)

**Responsabilidade**: Cliente

**Monitorização do Centro de Segurança Azure**: Nenhum

### <a name="76-securely-store-custom-operating-system-images"></a>7.6: Armazenar de forma segura imagens do sistema operativo personalizado

**Orientação**: Se utilizar imagens personalizadas, utilize o controlo de acesso baseado em funções Azure (Azure RBAC) para garantir que apenas os utilizadores autorizados possam aceder às imagens. Para imagens de contentores, guarde-as no Registo do Contentor Azure e aproveite o Azure RBAC para garantir que apenas os utilizadores autorizados possam aceder às imagens. 

- [Compreender Azure RBAC](../role-based-access-control/rbac-and-directory-admin-roles.md) 

- [Compreender O RBAC de Azure para o Registo de Contentores](../container-registry/container-registry-roles.md) 

- [Como configurar o Azure RBAC](../role-based-access-control/quickstart-assign-role-user-portal.md)

**Responsabilidade**: Cliente

**Monitorização do Centro de Segurança Azure**: Nenhum

### <a name="77-deploy-configuration-management-tools-for-azure-resources"></a>7.7: Implementar ferramentas de gestão de configuração para recursos Azure

**Orientação**: Utilize pseudónimos da Política Azure no espaço de nomes "Microsoft.ServiceFabric" para criar políticas personalizadas para alertar, auditar e impor configurações do sistema. Além disso, desenvolva um processo e um oleoduto para gerir exceções políticas.

- [Como configurar e gerir o Azure Policy](../governance/policy/tutorials/create-and-manage.md)

**Responsabilidade**: Cliente

**Monitorização do Centro de Segurança Azure**: Nenhum

### <a name="79-implement-automated-configuration-monitoring-for-azure-resources"></a>7.9: Implementar monitorização automatizada de configuração para recursos Azure

**Orientação**: Utilize pseudónimos da Política Azure no espaço de nomes "Microsoft.ServiceFabric" para criar políticas personalizadas para auditar ou impor a configuração do seu cluster de Tecido de Serviço.

- [Como ver pseudónimos disponíveis da Política Azure](https://docs.microsoft.com/powershell/module/az.resources/get-azpolicyalias?view=azps-4.8.0&amp;preserve-view=true)

- [Como configurar e gerir o Azure Policy](../governance/policy/tutorials/create-and-manage.md)

**Responsabilidade**: Cliente

**Monitorização do Centro de Segurança Azure**: Nenhum

### <a name="710-implement-automated-configuration-monitoring-for-operating-systems"></a>7.10: Implementar monitorização automatizada de configuração para sistemas operativos

**Orientação**: Utilize o Centro de Segurança para efetuar as verificações de base para as definições de OS e Docker para contentores. 

- [Compreender as recomendações do contentor do Centro de Segurança do Azure](../security-center/container-security.md)

**Responsabilidade**: Cliente

**Monitorização do Centro de Segurança Azure**: Nenhum

### <a name="711-manage-azure-secrets-securely"></a>7.11: Gerir os segredos do Azure de forma segura

**Orientação**: Utilize a Identidade de Serviço Gerido em conjunto com o Azure Key Vault para simplificar e garantir uma gestão secreta para as suas aplicações em nuvem. 

- [Utilização de identidades geridas para Azure com Tecido de Serviço](concepts-managed-identity.md)

- [Configurar suporte de identidade gerido para um novo cluster de Tecidos de Serviço](configure-new-azure-service-fabric-enable-managed-identity.md)

- [Utilize identidade gerida com uma aplicação de Tecido de Serviço](how-to-managed-identity-service-fabric-app-code.md)

- [Suporte keyVaultReference para aplicações de Tecido de Serviço](service-fabric-keyvault-references.md)

**Responsabilidade**: Cliente

**Monitorização do Centro de Segurança Azure**: Nenhum

### <a name="712-manage-identities-securely-and-automatically"></a>7.12: Gerir as identidades de forma segura e automática

**Orientação**: As identidades geridas podem ser utilizadas em clusters de tecidos de serviço implantados pelo Azure e para aplicações implantadas como recursos Azure. Identidades geridas permite-lhe autenticar qualquer serviço que suporte a autenticação do Azure Ative Directory (Azure AD), incluindo o Key Vault, sem quaisquer credenciais no seu código.

- [Utilização de identidades geridas para Azure com Tecido de Serviço](concepts-managed-identity.md)

**Responsabilidade**: Cliente

**Monitorização do Centro de Segurança Azure**: Nenhum

### <a name="713-eliminate-unintended-credential-exposure"></a>7.13: Eliminar a exposição credencial não intencional

**Orientação**: Se utilizar qualquer código relacionado com a sua implantação do Tecido de Serviço Azure, poderá implementar o Scanner credencial para identificar credenciais dentro do código. O Scanner de Credenciais também vai incentivar a movimentação das credenciais descobertas para localizações mais seguras, por exemplo, o Azure Key Vault.

Utilize o Cofre da Chave Azure para rodar automaticamente os certificados de cluster de tecido de serviço.

- [Como configurar o Scanner Credencial](https://secdevtools.azurewebsites.net/helpcredscan.html)

- [Gestão de certificados em clusters de tecidos de serviço](https://docs.microsoft.com/azure/service-fabric/cluster-security-certificate-management#certificate-rotation)

**Responsabilidade**: Cliente

**Monitorização do Centro de Segurança Azure**: Nenhum

## <a name="malware-defense"></a>Defesa contra Software maligno

*Para mais informações, consulte a [Referência de Segurança Azure: Defesa contra malware.](../security/benchmarks/security-control-malware-defense.md)*

### <a name="81-use-centrally-managed-anti-malware-software"></a>8.1: Utilize software anti-malware gerido centralmente

**Orientação**: Por padrão, o antivírus do Windows Defender é instalado no Windows Server 2016. A interface do utilizador é instalada por padrão em alguns SKUs, mas não é necessária.

Consulte a documentação antimalware para obter regras de configuração se não estiver a utilizar o Windows Defender. O Windows Defender não é suportado no Linux.

- [Compreender o Antivírus do Windows Defender no Windows Server 2016](/windows/security/threat-protection/windows-defender-antivirus/windows-defender-antivirus-on-windows-server-2016)

**Responsabilidade**: Cliente

**Monitorização do Centro de Segurança Azure**: Nenhum

## <a name="data-recovery"></a>Recuperação de Dados

*Para obter mais informações, consulte o [Benchmark de Segurança Azure: Recuperação de Dados](../security/benchmarks/security-control-data-recovery.md).*

### <a name="91-ensure-regular-automated-back-ups"></a>9.1: Garantir back ups automáticos regulares

**Orientação**: O serviço de backup e restauro no Tecido de Serviço permite uma cópia de segurança fácil e automática das informações armazenadas em serviços estatais. O backup dos dados da aplicação numa base periódica é fundamental para proteger contra a perda de dados e a indisponibilidade de serviço. O Service Fabric fornece um serviço de backup e restauro opcional, que lhe permite configurar a cópia de segurança periódica dos Serviços Fidedigdos (incluindo Serviços de Ator) sem ter de escrever nenhum código adicional. Também facilita a restauração de backups previamente tomadas.

- [Backup periódico e restauro em um cluster de tecido de serviço Azure](service-fabric-backuprestoreservice-quickstart-azurecluster.md)

**Responsabilidade**: Cliente

**Monitorização do Centro de Segurança Azure**: Nenhum

### <a name="92-perform-complete-system-backups-and-backup-any-customer-managed-keys"></a>9.2: Execute cópias de segurança completas do sistema e faça backups de backups de qualquer tecla gerida pelo cliente

**Orientação**: Ative o serviço de restauro de backup no seu cluster de Tecido de Serviço e crie políticas de backup para apoiar serviços estatais periodicamente e a pedido. Faça o reforço das chaves geridas pelo cliente dentro do Cofre da Chave Azure.

- [Backup periódico e restauro em um cluster de tecido de serviço Azure](service-fabric-backuprestoreservice-quickstart-azurecluster.md)

- [Compreensão da configuração de backup periódica no Tecido de Serviço Azure](service-fabric-backuprestoreservice-configure-periodic-backup.md)

- [Como backup chaves chave cofre em Azure](https://docs.microsoft.com/powershell/module/az.keyvault/backup-azkeyvaultkey?view=azps-4.8.0&amp;preserve-view=true)

**Responsabilidade**: Cliente

**Monitorização do Centro de Segurança Azure**: Nenhum

### <a name="93-validate-all-backups-including-customer-managed-keys"></a>9.3: Validar todas as cópias de segurança, incluindo chaves geridas pelo cliente

**Orientação**: Garantir a capacidade de efetuar a restauração a partir do serviço de restauro de backup, analisando periodicamente as informações de configuração de backup e as cópias de segurança disponíveis. Teste de restauração de chaves geridas pelo cliente.

- [Compreensão da configuração de backup periódica no Tecido de Serviço Azure](service-fabric-backuprestoreservice-configure-periodic-backup.md)

- [Restaurar a cópia de segurança no Tecido de Serviço Azure](service-fabric-backup-restore-service-trigger-restore.md)

- [Como restaurar chaves chave do cofre em Azure](https://docs.microsoft.com/powershell/module/az.keyvault/restore-azkeyvaultkey?view=azps-4.8.0&amp;preserve-view=true)

**Responsabilidade**: Cliente

**Monitorização do Centro de Segurança Azure**: Nenhum

### <a name="94-ensure-protection-of-backups-and-customer-managed-keys"></a>9.4: Garantir a proteção das cópias de segurança e das chaves geridas pelo cliente

**Orientação**: Cópias de segurança do serviço de backback service Fabric utilizam uma conta de Armazenamento Azure na sua subscrição. O Azure Storage encripta todos os dados numa conta de armazenamento em repouso. Por predefinição, os dados são encriptados com as teclas geridas pela Microsoft. Para controlo adicional sobre as chaves de encriptação, pode fornecer chaves geridas pelo cliente para encriptação de dados de armazenamento.

Se estiver a utilizar chaves geridas pelo cliente, certifique-se de Soft-Delete no Cofre de Chaves está ativado para proteger as chaves contra a eliminação acidental ou maliciosa.

- [Encriptação do Armazenamento do Azure em repouso](../storage/common/storage-service-encryption.md)

- [Como permitir Soft-Delete em Key Vault](../storage/blobs/soft-delete-blob-overview.md)

**Responsabilidade**: Cliente

**Monitorização do Centro de Segurança Azure**: Nenhum

## <a name="incident-response"></a>Resposta a Incidentes

*Para obter mais informações, veja [Referência de Segurança do Azure: Resposta a Incidentes](../security/benchmarks/security-control-incident-response.md).*

### <a name="101-create-an-incident-response-guide"></a>10.1: Criar um guia de resposta a incidentes

**Orientação**: Desenvolva um guia de resposta a incidentes para a sua organização. Certifique-se de que existem planos escritos de resposta a incidentes que definem todas as funções do pessoal, bem como as fases de tratamento e gestão de incidentes, desde a deteção até à revisão pós-incidente. 

- [Orientação para a construção do seu próprio processo de resposta a incidentes de segurança](https://msrc-blog.microsoft.com/2019/07/01/inside-the-msrc-building-your-own-security-incident-response-process/) 

- [Anatomia de um incidente do Microsoft Security Response Center](https://msrc-blog.microsoft.com/2019/06/27/inside-the-msrc-anatomy-of-a-ssirp-incident/) 

- [Utilize o Guia de Tratamento de Incidentes de Segurança Informática da NIST para ajudar na criação do seu próprio plano de resposta a incidentes](https://csrc.nist.gov/publications/detail/sp/800-61/rev-2/final)

**Responsabilidade**: Cliente

**Monitorização do Centro de Segurança Azure**: Nenhum

### <a name="102-create-an-incident-scoring-and-prioritization-procedure"></a>10.2: Criar um procedimento de pontuação e priorização de incidentes

**Orientação**: O Centro de Segurança atribui uma gravidade a cada alerta para o ajudar a priorizar quais os alertas que devem ser investigados primeiro. A gravidade baseia-se na confiança que o Centro de Segurança está na descoberta ou na métrica usada para emitir o alerta, bem como no nível de confiança de que havia intenção maliciosa por trás da atividade que levou ao alerta.

Além disso, marque subscrições usando tags e crie um sistema de nomeação para identificar e categorizar recursos Azure, especialmente aqueles que processam dados sensíveis.  É sua responsabilidade priorizar a reparação de alertas com base na criticidade dos recursos e ambiente do Azure onde o incidente ocorreu. 

- [Alertas de segurança no Centro de Segurança do Azure](../security-center/security-center-alerts-overview.md) 

- [Utilizar etiquetas para organizar os recursos do Azure](../azure-resource-manager/management/tag-resources.md)

**Responsabilidade**: Cliente

**Monitorização do Centro de Segurança Azure**: Nenhum

### <a name="103-test-security-response-procedures"></a>10.3: Procedimentos de resposta à segurança do teste

**Orientação**: Realize exercícios para testar as capacidades de resposta a incidentes dos seus sistemas numa cadência regular. Identifique pontos fracos e lacunas e reavalie o plano, conforme necessário. 

- [Guia NIST para programas de teste, formação e exercício para planos e capacidades de TI](https://nvlpubs.nist.gov/nistpubs/Legacy/SP/nistspecialpublication800-84.pdf)

**Responsabilidade**: Cliente

**Monitorização do Centro de Segurança Azure**: Nenhum

### <a name="104-provide-security-incident-contact-details-and-configure-alert-notifications-for-security-incidents"></a>10.4: Fornecer dados de contacto com incidentes de segurança e configurar notificações de alerta para incidentes de segurança

**Orientação**: As informações de contacto com incidentes de segurança serão utilizadas pela Microsoft para o contactar se o Microsoft Security Response Center (MSRC) descobrir que os seus dados foram acedidos por uma parte ilegal ou não autorizada. Reveja os incidentes após o facto de garantir que as questões sejam resolvidas. 

- [Como definir o contacto de segurança do Centro de Segurança do Azure](../security-center/security-center-provide-security-contact-details.md)

**Responsabilidade**: Cliente

**Monitorização do Centro de Segurança Azure**: Nenhum

### <a name="105-incorporate-security-alerts-into-your-incident-response-system"></a>10.5: Incorporar alertas de segurança no seu sistema de resposta a incidentes

**Orientação**: Exporte os alertas e recomendações do Seu Centro de Segurança utilizando a função Exportação Contínua. A Exportação Contínua permite-lhe exportar alertas e recomendações manualmente ou de forma contínua e contínua. Pode utilizar o conector de dados do Centro de Segurança para transmitir os alertas ao Sentinel.

- [Como configurar a exportação contínua](../security-center/continuous-export.md)

- [Como transmitir alertas para o Azure Sentinel](../sentinel/connect-azure-security-center.md)

**Responsabilidade**: Cliente

**Monitorização do Centro de Segurança Azure**: Nenhum

### <a name="106-automate-the-response-to-security-alerts"></a>10.6: Automatizar a resposta aos alertas de segurança

**Orientação**: Utilize a função de automatização do fluxo de trabalho no Centro de Segurança Azure para desencadear automaticamente respostas através de "Aplicações Lógicas" em alertas e recomendações de segurança.

- [Como configurar a automatização do fluxo de trabalho e as aplicações lógicas](../security-center/workflow-automation.md)

**Responsabilidade**: Cliente

**Monitorização do Centro de Segurança Azure**: Nenhum

## <a name="penetration-tests-and-red-team-exercises"></a>Testes de Penetração e Exercícios de Red Team

*Para obter mais informações, consulte o [Azure Security Benchmark: Testes de penetração e exercícios da equipa vermelha](../security/benchmarks/security-control-penetration-tests-red-team-exercises.md).*

### <a name="111-conduct-regular-penetration-testing-of-your-azure-resources-and-ensure-remediation-of-all-critical-security-findings"></a>11.1: Realizar testes regulares de penetração dos seus recursos Azure e garantir a reparação de todas as conclusões críticas de segurança

**Orientação**: Siga as regras de teste de penetração da Microsoft cloud para garantir que os seus testes de penetração não violam as políticas da Microsoft. Utilize a estratégia e a execução de "Equipas de Ataque" e os testes de penetração no local em direto da Microsoft na infraestrutura, nos serviços e nas aplicações cloud geridas pela Microsoft. 

- [Regras de Interação para os Testes de Penetração](https://www.microsoft.com/msrc/pentest-rules-of-engagement?rtc=1) 

- ["Equipa de Ataque" da Microsoft Cloud](https://gallery.technet.microsoft.com/Cloud-Red-Teaming-b837392e)

**Responsabilidade**: Partilhada

**Monitorização do Centro de Segurança Azure**: Nenhum

## <a name="next-steps"></a>Passos seguintes

- Veja a [Descrição geral da Referência de Segurança do Azure v2](/azure/security/benchmarks/overview)
- Saiba mais sobre as [linhas de base de segurança do Azure](/azure/security/benchmarks/security-baselines-overview)
