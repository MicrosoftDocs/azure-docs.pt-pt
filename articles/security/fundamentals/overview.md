---
title: Introdução à segurança Azure | Microsoft Docs
description: Apresente-se à Azure Security, aos seus vários serviços e ao funcionamento da leitura desta visão geral.
services: security
documentationcenter: na
author: TerryLanfear
manager: rkarlin
ms.assetid: ''
ms.service: security
ms.subservice: security-fundamentals
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/03/2021
ms.author: TomSh
ms.openlocfilehash: b5f9df4e6f682b5d1e9e3cd35affe6e4191e3d53
ms.sourcegitcommit: 32e0fedb80b5a5ed0d2336cea18c3ec3b5015ca1
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/30/2021
ms.locfileid: "105047784"
---
# <a name="introduction-to-azure-security"></a>Introdução à segurança do Azure

## <a name="overview"></a>Descrição Geral

Sabemos que a segurança é um trabalho na nuvem e como é importante que encontre informações precisas e oportunas sobre a segurança do Azure. Uma das melhores razões para usar o Azure para as suas aplicações e serviços é aproveitar a sua ampla gama de ferramentas e capacidades de segurança. Estas ferramentas e capacidades ajudam a tornar possível a criação de soluções seguras na plataforma Azure segura. O Microsoft Azure fornece confidencialidade, integridade e disponibilidade de dados dos clientes, ao mesmo tempo que permite uma responsabilização transparente.

Este artigo fornece um olhar abrangente sobre a segurança disponível com a Azure.

### <a name="azure-platform"></a>Plataforma Azure

O Azure é uma plataforma de serviços de nuvem pública que suporta uma ampla seleção de sistemas operativos, linguagens de programação, quadros, ferramentas, bases de dados e dispositivos. Pode executar contentores Linux com integração Docker; construir aplicativos com JavaScript, Python, .NET, PHP, Java e Node.js; construir back-ends para dispositivos iOS, Android e Windows.

Os serviços públicos de nuvem Azure suportam as mesmas tecnologias em que milhões de desenvolvedores e profissionais de TI já confiam e confiam. Quando você baseia ou migra ativos de TI para um prestador de serviços de nuvem pública você está confiando nas habilidades dessa organização para proteger suas aplicações e dados com os serviços e os controlos que eles fornecem para gerir a segurança dos seus ativos baseados na nuvem.

A infraestrutura da Azure é projetada desde instalações até aplicações para hospedar milhões de clientes simultaneamente, e fornece uma base confiável sobre a qual as empresas podem satisfazer os seus requisitos de segurança.

Além disso, o Azure fornece-lhe um vasto leque de opções de segurança configuráveis e a capacidade de controlá-las para que possa personalizar a segurança para satisfazer os requisitos únicos das implementações da sua organização. Este documento ajuda-o a compreender como as capacidades de segurança do Azure podem ajudá-lo a preencher estes requisitos.

> [!Note]
> O principal foco deste documento é nos controlos voltados para o cliente que pode utilizar para personalizar e aumentar a segurança das suas aplicações e serviços.
>
> Para obter informações sobre como a Microsoft assegura a própria plataforma Azure, consulte [a segurança da infraestrutura Azure](infrastructure.md).

## <a name="summary-of-azure-security-capabilities"></a>Resumo das capacidades de segurança da Azure

Dependendo do modelo de serviço na nuvem, existe responsabilidade variável para quem é responsável pela gestão da segurança da aplicação ou serviço. Existem capacidades disponíveis na Plataforma Azure para o ajudar a cumprir estas responsabilidades através de funcionalidades incorporadas e através de soluções parceiras que podem ser implantadas numa subscrição do Azure.

As capacidades incorporadas estão organizadas em seis áreas funcionais: Operações, Aplicações, Armazenamento, Networking, Computação e Identidade. Detalhes adicionais sobre as funcionalidades e capacidades disponíveis na Plataforma Azure nestas seis áreas são fornecidos através de informações sumárias.

## <a name="operations"></a>Operações

Esta secção fornece informações adicionais sobre as principais funcionalidades das operações de segurança e informações sumárias sobre estas capacidades.

### <a name="azure-security-center"></a>Centro de Segurança do Azure

[O Centro de Segurança](../../security-center/security-center-introduction.md) ajuda-o a prevenir, detetar e responder a ameaças com maior visibilidade e controlo sobre a segurança dos seus recursos Azure. Fornece gestão de políticas e monitorização de segurança integrada nas suas subscrições do Azure, ajuda a detetar ameaças que caso contrário podem passar despercebidas e funciona com um ecossistema abrangente de soluções de segurança.

Além disso, o Security Center ajuda nas operações de segurança, fornecendo-lhe um único painel de instrumentos que surface alertas e recomendações que podem ser atuados imediatamente. Muitas vezes, pode remediar problemas com um único clique dentro da consola do Centro de Segurança.

### <a name="azure-resource-manager"></a>Azure Resource Manager

[O Azure Resource Manager](../../azure-resource-manager/management/overview.md) permite-lhe trabalhar com os recursos na sua solução como grupo. Pode implementar, atualizar ou eliminar todos os recursos da sua solução numa operação única e coordenada. Você usa um [modelo de Gestor de Recursos Azure](../../azure-resource-manager/templates/overview.md) para implantação e esse modelo pode funcionar para diferentes ambientes, tais como testes, encenação e produção. O Resource Manager fornece funcionalidades de segurança, auditoria e etiquetagem para o ajudar a gerir os recursos após a implementação.

As implementações baseadas em modelos do Azure Resource Manager ajudam a melhorar a segurança das soluções implementadas no Azure porque as definições padrão de controlo de segurança e podem ser integradas em implementações padronizadas baseadas em modelos. Isto reduz o risco de erros de configuração de segurança que podem ocorrer durante as implementações manuais.

### <a name="application-insights"></a>Application Insights

[Application Insights](../../azure-monitor/app/app-insights-overview.md) é um serviço extensível de Gestão de Desempenho de Aplicações (APM) para desenvolvedores web. Com o Application Insights, pode monitorizar as suas aplicações web ao vivo e detetar automaticamente anomalias de desempenho. Inclui poderosas ferramentas de análise para ajudá-lo a diagnosticar problemas e a entender o que os utilizadores realmente fazem com as suas apps. Monitoriza a sua aplicação o tempo todo que está a funcionar, tanto durante os testes como depois de a ter publicado ou implantado.

O Application Insights cria gráficos e tabelas que lhe mostram, por exemplo, quais as horas do dia que obtém a maioria dos utilizadores, quão responsiva a aplicação é e quão bem é servida por quaisquer serviços externos de que depende.

Se houver falhas, falhas ou problemas de desempenho, pode pesquisar em detalhe os dados da telemetria para diagnosticar a causa. E o serviço envia-lhe e-mails se houver alguma alteração na disponibilidade e desempenho da sua app. A Application Insight torna-se assim uma valiosa ferramenta de segurança porque ajuda com a disponibilidade na tríade de confidencialidade, integridade e segurança de disponibilidade.

### <a name="azure-monitor"></a>Azure Monitor

[O Azure Monitor](/azure/monitoring-and-diagnostics/) oferece visualização, consulta, encaminhamento, alerta, escala automática e automação em dados tanto a partir da subscrição Azure ([Activity Log](../../azure-monitor/essentials/platform-logs-overview.md)) como cada recurso Azure individual[(Registos de Recursos).](../../azure-monitor/essentials/platform-logs-overview.md) Pode utilizar o Azure Monitor para alertá-lo sobre eventos relacionados com segurança que são gerados em registos Azure.

### <a name="azure-monitor-logs"></a>Registos do Azure Monitor

[Registos do Azure Monitor](../../azure-monitor/logs/log-query-overview.md) – Fornece uma solução de gestão de TI tanto para as infraestruturas baseadas em nuvem no local como para as infraestruturas baseadas em nuvem de terceiros (como a AWS) para além dos recursos da Azure. Os dados do Azure Monitor podem ser encaminhados diretamente para os registos do Azure Monitor para que possa ver métricas e registos para todo o seu ambiente num só local.

Os registos do Azure Monitor podem ser uma ferramenta útil na análise forense e outras análises de segurança, uma vez que a ferramenta permite pesquisar rapidamente através de grandes quantidades de entradas relacionadas com segurança com uma abordagem de consulta flexível. Além disso, os registos de firewall e proxy no local [podem ser exportados para Azure e disponibilizados para análise utilizando registos do Azure Monitor.](../../azure-monitor/agents/agent-windows.md)

### <a name="azure-advisor"></a>Assistente do Azure

[O Azure Advisor](../../advisor/advisor-overview.md) é um consultor de nuvem personalizado que o ajuda a otimizar as suas implementações do Azure. Analisa a configuração do recurso e a telemetria de utilização. Em seguida, recomenda soluções para ajudar a melhorar o [desempenho,](../../advisor/advisor-performance-recommendations.md) [segurança](../../advisor/advisor-security-recommendations.md)e [fiabilidade](../../advisor/advisor-high-availability-recommendations.md) dos seus recursos, enquanto procura oportunidades para [reduzir o seu gasto global do Azure.](../../advisor/advisor-cost-recommendations.md) O Azure Advisor fornece recomendações de segurança, que podem melhorar significativamente a sua postura de segurança global para as soluções que implementa no Azure. Estas recomendações são retiradas da análise de segurança realizada pelo [Azure Security Center.](../../security-center/security-center-introduction.md)

## <a name="applications"></a>Aplicações

A secção fornece informações adicionais sobre as principais funcionalidades na segurança da aplicação e informações sumárias sobre estas capacidades.

### <a name="web-application-vulnerability-scanning"></a>Digitalização da vulnerabilidade da Aplicação Web

Uma das formas mais fáceis de começar com testes para vulnerabilidades na sua [aplicação de Serviço de Aplicações](../../app-service/overview.md) é usar a integração com a [Tinfoil Security](https://azure.microsoft.com/blog/web-vulnerability-scanning-for-azure-app-service-powered-by-tinfoil-security/) para realizar uma pesquisa de vulnerabilidade de um clique na sua aplicação. Pode ver os resultados dos testes num relatório fácil de entender e aprender a corrigir cada vulnerabilidade com instruções passo a passo.

### <a name="penetration-testing"></a>Testes de Penetração

Não realizamos [testes](./pen-testing.md) de penetração da sua aplicação para si, mas entendemos que quer e precisa de realizar testes nas suas próprias aplicações. Isso é uma coisa boa, porque quando aumentas a segurança das tuas aplicações ajudas a tornar todo o ecossistema Azure mais seguro. Embora notificar a Microsoft das atividades de teste de canetas já não é necessário que os clientes continuem a cumprir [as regras de teste de penetração da Microsoft Cloud.](https://www.microsoft.com/msrc/pentest-rules-of-engagement)

### <a name="web-application-firewall"></a>Firewall de aplicação web

A firewall de aplicação web (WAF) em [Azure Application Gateway](../../application-gateway/features.md#web-application-firewall) ajuda a proteger aplicações web de ataques comuns baseados na web, como injeção de SQL, ataques de scripts de sites cruzados e sequestro de sessão. Vem pré-configurado com proteção contra ameaças identificadas pelo [Open Web Application Security Project (OWASP) como as 10 principais vulnerabilidades comuns.](https://www.owasp.org/index.php/Category:OWASP_Top_Ten_Project)

### <a name="authentication-and-authorization-in-azure-app-service"></a>Autenticação e autorização no Serviço de Aplicações do Azure

[A autenticação /Autorização do Serviço](../../app-service/overview-authentication-authorization.md) de Aplicações é uma funcionalidade que fornece uma forma de a sua aplicação iniciar sação nos utilizadores para que não tenha de alterar o código no backend da app. Fornece uma maneira fácil de proteger a sua aplicação e trabalhar com dados por utilizador.

### <a name="layered-security-architecture"></a>Arquitetura de Segurança Em camadas

Uma vez que [os Ambientes de Serviço de Aplicações](../../app-service/environment/app-service-app-service-environment-intro.md) fornecem um ambiente de tempo de execução isolado implantado numa [Rede Virtual Azure,](../../virtual-network/virtual-networks-overview.md)os desenvolvedores podem criar uma arquitetura de segurança em camadas que fornece diferentes níveis de acesso à rede para cada nível de aplicação. Um desejo comum é esconder as extremidades traseiras da API do acesso geral à Internet, e apenas permitir que as APIs sejam chamadas por aplicações web a montante. [Os grupos de segurança da rede (NSGs)](../../virtual-network/virtual-network-vnet-plan-design-arm.md) podem ser utilizados nas sub-redes da Rede Virtual Azure que contêm Ambientes de Serviço de Aplicações para restringir o acesso do público a aplicações API.

### <a name="web-server-diagnostics-and-application-diagnostics"></a>Diagnósticos de servidores web e diagnósticos de aplicações
[As aplicações web do Serviço de Aplicações](../../app-service/troubleshoot-diagnostic-logs.md) fornecem funcionalidade de diagnóstico para registar informações tanto do servidor web como da aplicação web. Estes são logicamente separados em diagnósticos de servidores web e diagnósticos de aplicações. O servidor web inclui dois grandes avanços no diagnóstico e resolução de problemas de sites e aplicações.

A primeira novidade é a informação do estado em tempo real sobre piscinas de aplicações, processos de trabalho, sites, domínios de aplicações e pedidos de execução. As segundas novas vantagens são os resultados detalhados que acompanham um pedido durante todo o processo completo de pedido e resposta.

Para permitir a recolha destes eventos de rastreio, o IIS 7 pode ser configurado para capturar automaticamente registos completos de vestígios, em formato XML, para qualquer pedido específico baseado em códigos de tempo ou resposta de erro decorridos.

## <a name="storage"></a>Armazenamento
A secção fornece informações adicionais sobre as principais funcionalidades da segurança de armazenamento Azure e informações sumárias sobre estas capacidades.

### <a name="azure-role-based-access-control-azure-rbac"></a>Controlo de acesso baseado em funções do Azure (RBAC do Azure)
Pode proteger a sua conta de armazenamento com [o controlo de acesso baseado em funções Azure (Azure RBAC)](../../role-based-access-control/overview.md). Restringir o acesso com base na [necessidade de conhecer](https://en.wikipedia.org/wiki/Need_to_know) e menos [privilégios](https://en.wikipedia.org/wiki/Principle_of_least_privilege) princípios de segurança é imperativo para as organizações que querem impor políticas de segurança para o acesso aos dados. Estes direitos de acesso são concedidos atribuindo o papel adequado de Azure a grupos e aplicações num determinado âmbito. Pode utilizar [funções incorporadas do Azure,](../../role-based-access-control/built-in-roles.md)como o Storage Account Contributor, para atribuir privilégios aos utilizadores. O acesso às chaves de armazenamento de uma conta de armazenamento utilizando o modelo [Azure Resource Manager](../../storage/blobs/security-recommendations.md#data-protection) pode ser controlado através do Azure RBAC.

### <a name="shared-access-signature"></a>Assinatura de acesso compartilhado
As [assinaturas de acesso partilhado (SAS)](../../storage/common/storage-sas-overview.md) disponibilizam acesso delegado a recursos na sua conta de armazenamento. O SAS significa que pode conceder a um cliente permissões limitadas a objetos na sua conta de armazenamento por um período especificado e com um conjunto especificado de permissões. Pode conceder estas permissões limitadas sem ter de partilhar as chaves de acesso à sua conta.

### <a name="encryption-in-transit"></a>Encriptação em Trânsito
A encriptação em trânsito é um mecanismo de proteção dos dados quando são transmitidos através de redes. Com o Azure Storage, pode proteger os dados utilizando:
- [Encriptação ao nível do transporte](../../storage/blobs/security-recommendations.md), como HTTPS quando transfere dados para dentro ou para fora do Azure Storage.

- [Encriptação de](../../storage/blobs/security-recommendations.md)fios , como [encriptação SMB 3.0](../../storage/blobs/security-recommendations.md) para [ações do Ficheiro Azure](../../storage/files/storage-dotnet-how-to-use-files.md).

- Encriptação do lado do cliente, para encriptar os dados antes de serem transferidos para o armazenamento e para desencriptar os dados após a sua transferência para fora do armazenamento.

### <a name="encryption-at-rest"></a>Encriptação de dados inativos
Para muitas organizações, a encriptação de dados em repouso é um passo obrigatório para a privacidade dos dados, conformidade e soberania de dados. Existem três funcionalidades de segurança de armazenamento Azure que fornecem encriptação de dados que estão "em repouso":

- [A Encriptação do Serviço de Armazenamento](../../storage/common/storage-service-encryption.md) permite-lhe solicitar que o serviço de armazenamento criptografe automaticamente os dados ao escrevê-lo para o Azure Storage.

- [A encriptação do lado do cliente](../../storage/common/storage-client-side-encryption.md) também fornece a funcionalidade de encriptação em repouso.

- [A encriptação do disco Azure](./azure-disk-encryption-vms-vmss.md) permite encriptar os discos de SISTEMA e discos de dados utilizados por uma máquina virtual IaaS.

### <a name="storage-analytics"></a>Análise de Armazenamento

[A Azure Storage Analytics](/rest/api/storageservices/fileservices/storage-analytics) realiza o registo e fornece dados de métricas para uma conta de armazenamento. Pode utilizar estes dados para rastrear pedidos, analisar tendências de utilização e diagnosticar problemas com a sua conta de armazenamento. A Análise de Armazenamento regista informações detalhadas sobre os pedidos com êxito e com falha feitos a um serviço de armazenamento. Estas informações podem ser utilizadas para monitorizar os pedidos individuais e diagnosticar problemas num serviço de armazenamento. Os pedidos são registados numa base de melhor esforço. São registados os seguintes tipos de pedidos autenticados:

- Pedidos bem sucedidos.
- Pedidos falhados, incluindo tempo limite, estrangulamento, rede, autorização e outros erros.
- Pedidos utilizando uma Assinatura de Acesso Partilhado (SAS), incluindo pedidos falhados e bem sucedidos.
- Pedidos de dados de análise.

### <a name="enabling-browser-based-clients-using-cors"></a>Habilitando Browser-Based clientes usando CORS

[Cross-Origin Resource Sharing (CORS)](/rest/api/storageservices/fileservices/cross-origin-resource-sharing--cors--support-for-the-azure-storage-services) é um mecanismo que permite que os domínios dêem permissão uns aos outros para acederem aos recursos uns dos outros. O Agente utilizador envia cabeçalhos extra para garantir que o código JavaScript carregado de um determinado domínio é autorizado a aceder a recursos localizados noutro domínio. Este último domínio responde então com cabeçalhos extras permitindo ou negando o acesso original do domínio aos seus recursos.

Os serviços de armazenamento Azure suportam agora o CORS de modo a que, uma vez definidas as regras CORS para o serviço, seja avaliado um pedido devidamente autenticado contra o serviço de um domínio diferente para determinar se é permitido de acordo com as regras especificadas.

## <a name="networking"></a>Rede

A secção fornece informações adicionais sobre as principais funcionalidades da segurança da rede Azure e informações sumárias sobre estas capacidades.

### <a name="network-layer-controls"></a>Controlos da camada de rede

O controlo do acesso à rede é o ato de limitar a conectividade de e para dispositivos ou subesí redes específicos e representa o núcleo da segurança da rede. O objetivo do controlo de acesso à rede é garantir que as suas máquinas e serviços virtuais estejam acessíveis apenas aos utilizadores e dispositivos a que os queira acessíveis.

#### <a name="network-security-groups"></a>Grupos de Segurança de Rede

Um [Grupo de Segurança de Rede (NSG)](../../virtual-network/virtual-network-vnet-plan-design-arm.md#security) é uma firewall de filtragem de pacotes básicos e imponente e permite-lhe controlar o acesso com base num 5-tuple. Os NSGs não fornecem inspeção da camada de aplicação ou controlos de acesso autenticados. Podem ser utilizados para controlar o tráfego que se desloca entre sub-redes dentro de uma Rede Virtual Azure e o tráfego entre uma Rede Virtual Azure e a Internet.

#### <a name="route-control-and-forced-tunneling"></a>Controlo de rotas e túneis forçados

A capacidade de controlar o comportamento de encaminhamento nas suas Redes Virtuais Azure é uma capacidade crítica de segurança da rede e controlo de acesso. Por exemplo, se quiser certificar-se de que todo o tráfego de e para a sua Rede Virtual Azure passa por esse aparelho de segurança virtual, precisa de ser capaz de controlar e personalizar o comportamento de encaminhamento. Pode fazê-lo configurando User-Defined Rotas em Azure.

[As Rotas Definidas pelo utilizador permitem-lhe](../../virtual-network/virtual-networks-udr-overview.md#custom-routes) personalizar caminhos de entrada e saída para o tráfego que se desloca dentro e fora de máquinas virtuais individuais ou sub-redes para garantir a rota mais segura possível. [O túnel forçado](../../vpn-gateway/vpn-gateway-forced-tunneling-rm.md) é um mecanismo que pode utilizar para garantir que os seus serviços não estão autorizados a iniciar uma ligação a dispositivos na Internet.

Isto é diferente de ser capaz de aceitar ligações recebidas e, em seguida, responder a elas. Os servidores web front-end precisam de responder a pedidos de anfitriões da Internet, e assim o tráfego de origem na Internet é permitido a entrada nestes servidores web e os servidores da web podem responder.

Os túneis forçados são geralmente usados para forçar o tráfego de saída para a Internet a passar por proxies de segurança no local e firewalls.

#### <a name="virtual-network-security-appliances"></a>Aparelhos de segurança de rede virtual

Enquanto grupos de segurança de rede, User-Defined Rotas e túneis forçados fornecem-lhe um nível de segurança na rede e camadas de transporte do [modelo OSI,](https://en.wikipedia.org/wiki/OSI_model)pode haver momentos em que pretende permitir a segurança em níveis mais altos da stack. Pode aceder a estas funcionalidades de segurança de rede melhoradas utilizando uma solução de sistema de segurança de rede de parceiros Azure. Pode encontrar as soluções de segurança de rede de parceiros Azure mais atuais, visitando o [Azure Marketplace](https://azure.microsoft.com/marketplace/) e procurando "segurança" e "segurança na rede".

### <a name="azure-virtual-network"></a>Rede Virtual do Azure

Uma rede virtual do Azure (VNet) é uma representação da sua própria rede na nuvem. É um isolamento lógico do tecido da rede Azure dedicado à sua subscrição. Pode controlar totalmente os blocos de endereços IP, as definições de DNS, as políticas de segurança e as tabelas de rotas dentro desta rede. Pode segmentar o seu VNet em sub-redes e colocar máquinas virtuais Azure IaaS (VMs) e/ou [cloud (instâncias de função PaaS)](../../cloud-services/cloud-services-choose-me.md) em Redes Virtuais Azure.

Além disso, pode ligar a rede virtual à sua rede no local através de uma das [opções de conectividade](../../vpn-gateway/index.yml) disponíveis no Azure. Essencialmente, pode expandir a sua rede para o Azure, com controlo total sobre blocos de endereços IP com a vantagem da escala empresarial que o Azure oferece.

A ligação em rede Azure suporta vários cenários de acesso remoto seguros. Algumas delas incluem:

- [Ligue estações de trabalho individuais a uma Rede Virtual Azure](../../vpn-gateway/vpn-gateway-howto-point-to-site-rm-ps.md)

- [Ligue a rede no local a uma Rede Virtual Azure com uma VPN](../../vpn-gateway/vpn-gateway-about-vpngateways.md)

- [Ligue a rede no local a uma Rede Virtual Azure com uma ligação WAN dedicada](../../expressroute/expressroute-introduction.md)

- [Ligue as redes virtuais do Azure entre si](../../vpn-gateway/vpn-gateway-vnet-vnet-rm-ps.md)

### <a name="azure-private-link"></a>Azure Private Link

[O Azure Private Link](https://azure.microsoft.com/services/private-link/) permite-lhe aceder aos Serviços Azure PaaS (por exemplo, Azure Storage e SQL Database) e à Azure aloja serviços de propriedade do cliente/parceiro privados na sua rede virtual sobre um [ponto final privado.](../../private-link/private-endpoint-overview.md) A configuração e consumo usando o Azure Private Link é consistente em todos os serviços Azure PaaS, propriedade do cliente e parceiros partilhados. O tráfego da sua rede virtual para o serviço Azure permanece sempre na rede de espinha dorsal do Microsoft Azure.

[Os Private Endpoints](../../private-link/private-endpoint-overview.md) permitem-lhe assegurar os seus recursos críticos de serviço Azure apenas às suas redes virtuais. O Azure Private Endpoint utiliza um endereço IP privado do seu VNet para o ligar de forma privada e segura a um serviço alimentado pela Azure Private Link, efetivamente trazendo o serviço para o seu VNet. Expor a sua rede virtual à internet pública já não é necessário para consumir serviços no Azure. 

Também pode criar o seu próprio serviço de ligação privada na sua rede virtual. [O serviço Azure Private Link](../../private-link/private-link-service-overview.md) é a referência ao seu próprio serviço que é alimentado pela Azure Private Link. O seu serviço que está a funcionar atrás do Azure Standard Load Balancer pode ser ativado para acesso private Link para que os consumidores do seu serviço possam aceder ao mesmo a partir das suas próprias redes virtuais. Os seus clientes podem criar um ponto final privado dentro da sua rede virtual e mapeá-lo para este serviço. Expor o seu serviço à internet pública já não é necessário prestar serviços no Azure. 

### <a name="vpn-gateway"></a>Gateway de VPN

Para enviar tráfego de rede entre a sua Rede Virtual Azure e o seu site no local, tem de criar uma porta de entrada VPN para a sua Rede Virtual Azure. Um [gateway VPN](../../vpn-gateway/vpn-gateway-about-vpngateways.md) é um tipo de gateway de rede virtual que envia tráfego encriptado através de uma ligação pública. Também pode utilizar gateways VPN para enviar tráfego entre redes virtuais Azure sobre o tecido da rede Azure.

### <a name="express-route"></a>ExpressRoute

O Microsoft Azure [ExpressRoute](../../expressroute/expressroute-introduction.md) é um link WAN dedicado que permite estender as suas redes no local para a nuvem da Microsoft através de uma ligação privada dedicada facilitada por um fornecedor de conectividade.

![ExpressRoute](./media/overview/azure-security-figure-1.png)

Com o ExpressRoute, pode estabelecer ligações aos serviços de cloud da Microsoft, tais como Microsoft Azure, Microsoft 365 e CRM Online. A conectividade pode ser a partir de uma rede qualquer a qualquer (VPN de IP), uma rede Ethernet de ponto a ponto ou uma ligação cruzada virtual através de um fornecedor de conectividade numa localização conjunta.

As ligações ExpressRoute não passam pela Internet pública e, portanto, podem ser consideradas mais seguras do que soluções baseadas em VPN. Tal permite que as ligações do ExpressRoute ofereçam mais fiabilidade, velocidades superiores, latências inferiores e uma maior segurança do que as ligações típicas através da Internet.

### <a name="application-gateway"></a>Gateway de Aplicação

O Microsoft [Azure Application Gateway](../../application-gateway/overview.md) fornece um [Controlador de Entrega de Aplicações (ADC)](https://en.wikipedia.org/wiki/Application_delivery_controller) como um serviço, oferecendo várias capacidades de equilíbrio de carga de camada 7 para a sua aplicação.

![Gateway de Aplicação](./media/overview/azure-security-figure-2.png)

Permite-lhe otimizar a produtividade da web farm descarregando a rescisão intensiva de CPU TLS para o Gateway de aplicação (também conhecido como "descarga TLS&quot; ou &quot;Ponte TLS"). Também fornece outras capacidades de encaminhamento da Camada 7, incluindo distribuição de rodada de tráfego de entrada, afinidade da sessão baseada em cookies, encaminhamento baseado em caminhos de URL e a capacidade de hospedar vários websites por trás de um único Gateway de aplicação. O Application Gateway do Azure é um balanceador de carga de 7 camadas.

Fornece ativação pós-falha, pedidos HTTP de encaminhamento de desempenho entre diversos servidores, quer estejam na nuvem ou no local.

A aplicação fornece muitas funcionalidades do Controlador de Entrega de Aplicações (ADC), incluindo equilíbrio de carga HTTP, afinidade da sessão baseada em [cookies, descarregamento de TLS,](../../web-application-firewall/ag/tutorial-restrict-web-traffic-powershell.md)sondas de saúde personalizadas, suporte para vários sites, e muitos outros.

### <a name="web-application-firewall"></a>Firewall de Aplicações Web

Web Application Firewall é uma característica do [Azure Application Gateway](../../application-gateway/overview.md) que fornece proteção para aplicações web que usam o gateway de aplicação para funções padrão de Controlo de Entrega de Aplicações (ADC). A Firewall de aplicações Web fá-lo ao protegê-las contra a maioria das 10 principais vulnerabilidades Web da OWASP.

![Firewall de Aplicações Web](./media/overview/azure-security-figure-3.png)

- Proteção contra injeção de SQL

- Proteção contra Ataques Web comuns, como, por exemplo, injeção de comandos, contrabando de pedidos HTTP, divisão de respostas HTTP e ataques remotos de inclusão de ficheiros

- Proteção contra violações de protocolo HTTP

- Proteção contra anomalias de protocolo HTTP, como agente de utilizador de anfitrião e cabeçalhos de aceitação em falta

- Prevenção de contra bots, crawlers e scanners

- Deteção de configurações comuns de aplicações (isto é, Apache, IIS, etc.)

Uma firewall de aplicações Web centralizada contra ataques Web simplifica em muito a gestão da segurança e confere à aplicação uma maior garantia de proteção contra as ameaças de intrusão. Uma solução WAF também pode reagir mais rapidamente a uma ameaça de segurança ao corrigir uma vulnerabilidade conhecida numa localização central, em vez de proteger cada uma das aplicações Web individualmente. Os gateways de aplicação existentes podem ser convertidos num gateway de aplicação com a firewall de aplicações Web facilmente.

### <a name="traffic-manager"></a>Gestor de Tráfego

O Microsoft [Azure Traffic Manager](../../traffic-manager/traffic-manager-overview.md) permite-lhe controlar a distribuição do tráfego de utilizadores para pontos finais de serviço em diferentes centros de dados. Os pontos finais de serviço suportados pelo Traffic Manager incluem VMs Azure, Web Apps e serviços Cloud. Também pode utilizar o Gestor de Tráfego com pontos finais não Azure externos. O Gestor de Tráfego utiliza o Sistema de Nome de Domínio (DNS) para direcionar os pedidos do cliente para o ponto final mais adequado, com base num [método de encaminhamento de tráfego](../../traffic-manager/traffic-manager-routing-methods.md) e na saúde dos pontos finais.

O Traffic Manager fornece uma gama de métodos de encaminhamento de tráfego para atender às diferentes necessidades de aplicação, [monitorização de](../../traffic-manager/traffic-manager-monitoring.md)saúde no ponto final e falha automática. O Gestor de Tráfego é resiliente a falhas, incluindo a falhas numa região do Azure inteira.

### <a name="azure-load-balancer"></a>Balanceador de Carga do Azure

O [Balanceador de Carga do Azure](../../load-balancer/load-balancer-overview.md) oferece elevada disponibilidade e elevado desempenho de rede às suas aplicações. Trata-se de um equilibrador de carga da Camada 4 (TCP, UDP) que distribui o tráfego de entrada entre instâncias saudáveis de serviços definidos num conjunto equilibrado de carga. O Equilibrador de Carga Azure pode ser configurado para:

- Carregar o equilíbrio que chega ao tráfego de Internet para máquinas virtuais. Esta configuração é conhecida como [equilíbrio de carga pública.](../../load-balancer/components.md#frontend-ip-configurations)

- Carregue o tráfego entre máquinas virtuais numa rede virtual, entre máquinas virtuais em serviços em nuvem, ou entre computadores no local e máquinas virtuais numa rede virtual transversal. Esta configuração é conhecida como [equilíbrio de carga interna](../../load-balancer/components.md#frontend-ip-configurations).

- Encaminhar o tráfego externo para uma máquina virtual específica

### <a name="internal-dns"></a>DNS internos

Pode gerir a lista de servidores DNS utilizados num VNet no Portal de Gestão ou no ficheiro de configuração da rede. O cliente pode adicionar até 12 servidores DNS para cada VNet. Ao especificar servidores DNS, é importante verificar se lista os servidores DNS do cliente na ordem correta para o ambiente do cliente. As listas de servidores DNS não funcionam redondamente. São utilizados na ordem que são especificadas. Se o primeiro servidor DNS da lista for capaz de ser alcançado, o cliente utiliza esse servidor DNS independentemente de o servidor DNS estar a funcionar corretamente ou não. Para alterar a encomenda do servidor DNS para a rede virtual do cliente, remova os servidores DNS da lista e adicione-os de volta na ordem que o cliente deseja. O DNS suporta o aspeto de disponibilidade da tríade de segurança "CIA".

### <a name="azure-dns"></a>DNS do Azure

O Sistema de Nome de Domínio, ou DNS, é responsável por traduzir (ou resolver) um nome de website ou serviço para o seu endereço IP. O [DNS do Azure](../../dns/dns-overview.md) é um serviço de alojamento dos domínios DNS que fornece resolução de nomes através da infraestrutura do Microsoft Azure. Ao alojar os seus domínios no Azure, pode gerir os recursos DNS com as mesmas credenciais, APIs, ferramentas e faturação dos seus outros serviços do Azure. O DNS suporta o aspeto de disponibilidade da tríade de segurança "CIA".

### <a name="azure-monitor-logs-nsgs"></a>Azure Monitor regista NSGs

Pode ativar as seguintes categorias de registo de diagnóstico para NSGs:

- Evento: Contém entradas para as quais as regras NSG são aplicadas a VMs e funções de instância com base no endereço MAC. O estatuto destas regras é recolhido a cada 60 segundos.

- Contador de regras: Contém entradas para quantas vezes cada regra NSG é aplicada para negar ou permitir o tráfego.

### <a name="security-center"></a>Centro de Segurança

[O Azure Security Center](../../security-center/security-center-introduction.md) analisa continuamente o estado de segurança dos seus recursos Azure para as melhores práticas de segurança da rede. Quando o Security Center identifica potenciais vulnerabilidades de segurança, cria [recomendações](../../security-center/security-center-recommendations.md) que o orientam através do processo de configuração dos controlos necessários para endurecer e proteger os seus recursos.

## <a name="compute"></a>Computação
A secção fornece informações adicionais sobre as principais funcionalidades nesta área e informações sumárias sobre estas capacidades.

### <a name="antimalware--antivirus"></a>Antimalware & Antivírus
Com o Azure IaaS, pode utilizar software antimalware de fornecedores de segurança como Microsoft, Symantec, Trend Micro, McAfee e Kaspersky para proteger as suas máquinas virtuais de ficheiros maliciosos, adware e outras ameaças. [O Microsoft Antimalware](antimalware.md) para Azure Cloud Services e Virtual Machines é uma capacidade de proteção que ajuda a identificar e remover vírus, spyware e outros softwares maliciosos. O Microsoft Antimalware fornece alertas configuráveis quando o software malicioso ou indesejado tenta instalar-se ou funcionar nos seus sistemas Azure. O Microsoft Antimalware também pode ser implementado usando o Azure Security Center

### <a name="hardware-security-module"></a>Módulo de Segurança de Hardware
A encriptação e a autenticação não melhoram a segurança a menos que as próprias chaves estejam protegidas. Pode simplificar a gestão e segurança dos seus segredos e chaves críticos armazenando-os no [Azure Key Vault.](../../key-vault/general/overview.md) O Key Vault oferece a opção de armazenar as suas chaves em módulos de segurança de hardware (HSMs) certificados para os padrões FIPS 140-2 Nível 2. As chaves de encriptação do SQL Server para encriptação de [dados de](/sql/relational-databases/security/encryption/transparent-data-encryption) backup ou transparente podem ser armazenadas no Key Vault com quaisquer chaves ou segredos das suas aplicações. As permissões e o acesso a estes itens protegidos são geridos através [do Azure Ative Directory](https://azure.microsoft.com/documentation/services/active-directory/).

### <a name="virtual-machine-backup"></a>Backup de máquina virtual
[O Azure Backup](../../backup/backup-overview.md) é uma solução que protege os dados da sua aplicação com investimento de capital zero e custos operacionais mínimos. Os erros de aplicação podem corromper os seus dados, e os erros humanos podem introduzir bugs nas suas aplicações que podem levar a problemas de segurança. Com o Azure Backup, as suas máquinas virtuais que executam o Windows e o Linux estão protegidas.

### <a name="azure-site-recovery"></a>Azure Site Recovery
Uma parte importante da estratégia de [continuidade de negócios/recuperação](../../best-practices-availability-paired-regions.md) de desastres da sua organização (BCDR) é descobrir como manter as cargas de trabalho corporativas e apps em funcionamento quando ocorrem interrupções planeadas e não planeadas. [A Azure Site Recovery](../../site-recovery/site-recovery-overview.md) ajuda a orquestrar a replicação, failover e a recuperação de cargas de trabalho e aplicações para que estejam disponíveis a partir de um local secundário se a sua localização primária diminuir.

### <a name="sql-vm-tde"></a>SQL VM TDE
Encriptação transparente de dados (TDE) e encriptação de nível de coluna (CLE) são funcionalidades de encriptação do servidor SQL. Esta forma de encriptação requer que os clientes gerem e armazenem as chaves criptográficas que utiliza para encriptação.

O serviço Azure Key Vault (AKV) foi concebido para melhorar a segurança e a gestão destas chaves num local seguro e altamente disponível. O Conector do Servidor SQL permite que o SQL Server utilize estas chaves a partir do Cofre da Chave Azure.

Se estiver a executar o SQL Server com máquinas no local, existem passos que pode seguir para aceder ao Cofre da Chave Azure a partir da sua instância do SQL Server no local. Mas para o SQL Server em VMs Azure, pode economizar tempo utilizando a função de Integração do Cofre de Chave Azure. Com alguns cmdlets Azure PowerShell para ativar esta função, pode automatizar a configuração necessária para que um SQL VM aceda ao seu cofre de chaves.

### <a name="vm-disk-encryption"></a>Encriptação do disco VM
[A Azure Disk Encryption](./azure-disk-encryption-vms-vmss.md) é uma nova capacidade que o ajuda a encriptar os discos de máquina virtual Windows e Linux IaaS. Aplica a funcionalidade BitLocker padrão da indústria do Windows e a funcionalidade DM-Crypt do Linux para fornecer encriptação de volume para o SISTEMA e os discos de dados. A solução está integrada com o Azure Key Vault para o ajudar a controlar e gerir as chaves e segredos de encriptação do disco na sua subscrição do Key Vault. A solução também garante que todos os dados dos discos de máquinas virtuais são encriptados em repouso no seu armazenamento Azure.

### <a name="virtual-networking"></a>Redes virtuais
As máquinas virtuais precisam de conectividade de rede. Para suportar este requisito, o Azure requer que as máquinas virtuais sejam ligadas a uma Rede Virtual Azure. Uma Rede Virtual Azure é uma construção lógica construída em cima do tecido físico da rede Azure. Cada [Rede Virtual Azure](../../virtual-network/virtual-networks-overview.md) lógica está isolada de todas as outras Redes Virtuais Azure. Este isolamento ajuda a garantir que o tráfego de rede nas suas implementações não seja acessível a outros clientes da Microsoft Azure.

### <a name="patch-updates"></a>Atualizações de patch
As Atualizações de Patch fornecem a base para encontrar e corrigir potenciais problemas e simplificar o processo de gestão de atualização de software, tanto reduzindo o número de atualizações de software que deve implementar na sua empresa como aumentando a sua capacidade de monitorizar a conformidade.

### <a name="security-policy-management-and-reporting"></a>Gestão e reporte de políticas de segurança
[O Centro de Segurança](../../security-center/security-center-introduction.md) ajuda-o a prevenir, detetar e responder a ameaças, e fornece-lhe uma maior visibilidade e controlo sobre a segurança dos seus recursos Azure. Fornece monitorização integrada de segurança e gestão de políticas em todas as suas subscrições Azure, ajuda a detetar ameaças que de outra forma poderiam passar despercebidas, e trabalha com um amplo ecossistema de soluções de segurança.

## <a name="identity-and-access-management"></a>Gestão de identidades e acessos
A segurança de sistemas, aplicações e dados começa com controlos de acesso baseados na identidade. As funcionalidades de gestão de identidade e acesso que são incorporadas em produtos e serviços empresariais da Microsoft ajudam a proteger as suas informações organizacionais e pessoais de acesso não autorizado, disponibilizando-as a utilizadores legítimos sempre e onde precisarem.

### <a name="secure-identity"></a>Identidade Segura
A Microsoft utiliza múltiplas práticas e tecnologias de segurança em todos os seus produtos e serviços para gerir a identidade e o acesso.

- [A Autenticação Multi-Factor](https://azure.microsoft.com/services/multi-factor-authentication/) requer que os utilizadores utilizem vários métodos de acesso, no local e na nuvem. Proporciona uma autenticação forte com um leque de opções de verificação fáceis, ao mesmo tempo que acomoda os utilizadores com um simples processo de entrada.

- [O Microsoft Authenticator](https://aka.ms/authenticator) fornece uma experiência de autenticação multi-factor fácil de utilizar que funciona tanto com as contas microsoft Azure Ative Directory como com as contas da Microsoft, e inclui suporte para wearables e aprovações baseadas em impressões digitais.

- [A aplicação da política de palavras-passe](../../active-directory/authentication/concept-sspr-policy.md) aumenta a segurança das palavras-passe tradicionais, impondo requisitos de comprimento e complexidade, rotação periódica forçada e bloqueio de conta após tentativas falhadas de autenticação.

- [A autenticação baseada em token](../../active-directory/develop/authentication-vs-authorization.md) permite a autenticação através do Azure Ative Directory.

- [O controlo de acesso baseado em funções (Azure RBAC) permite-lhe](../../role-based-access-control/built-in-roles.md) conceder acesso com base na função atribuída ao utilizador, facilitando aos utilizadores apenas a quantidade de acesso de que necessitam para desempenharem as suas funções. Você pode personalizar Azure RBAC de acordo com o modelo de negócio da sua organização e tolerância ao risco.

- [A gestão integrada da identidade (identidade híbrida)](../../active-directory/hybrid/plan-hybrid-identity-design-considerations-overview.md) permite manter o controlo do acesso dos utilizadores através de datacenters internos e plataformas cloud, criando uma identidade única de utilizador para a autenticação e autorização de todos os recursos.

### <a name="secure-apps-and-data"></a>Aplicações e dados seguros
[O Azure Ative Directory](https://azure.microsoft.com/services/active-directory/), uma solução abrangente de nuvem de gestão de identidade e acesso, ajuda a garantir o acesso aos dados em aplicações no site e na nuvem, e simplifica a gestão dos utilizadores e grupos. Combina serviços de diretórios centrais, governação avançada de identidade, segurança e gestão de acesso a aplicações, e facilita aos desenvolvedores a construção de gestão de identidade baseada em políticas nas suas apps. Para otimizar o Azure Active Directory, pode adicionar capacidades pagas com as edições Azure Active Directory Básico, Premium P1 e Premium P2.

| Características gratuitas / comuns     | Características Básicas    |Características premium P1 |Características premium P2 | Azure Ative Directory Join – funcionalidades apenas relacionadas com o Windows 10|
| :------------- | :------------- |:------------- |:------------- |:------------- |
|   [Objetos de diretório](../../active-directory/fundamentals/active-directory-whatis.md),    [Gestão do Utilizador/Grupo (adicionar/atualizar/excluir)/ Fornecimento baseado no utilizador, registo do dispositivo,](../../active-directory/fundamentals/active-directory-whatis.md)  [Sign-On único (SSO)](../../active-directory/fundamentals/active-directory-whatis.md), Alteração da     [palavra-passe de autosserviço para utilizadores na nuvem,](../../active-directory/fundamentals/active-directory-whatis.md)     [Connect (motor de sincronização que estende os diretórios no local ao Diretório Ativo Azure)](../../active-directory/fundamentals/active-directory-whatis.md),     [Relatórios de Segurança / Utilização](../../active-directory/fundamentals/active-directory-whatis.md)       |  [Gestão /provisionamento de acessos baseados em grupo,](../../active-directory/fundamentals/active-directory-whatis.md) [Autosserviço Password Reset para utilizadores em nuvem,](../../active-directory/fundamentals/active-directory-whatis.md)  [Branding da Empresa (Página de Logon/Personalização do Painel de Acesso)](../../active-directory/fundamentals/active-directory-whatis.md),    [Procuração de Aplicações](../../active-directory/fundamentals/active-directory-whatis.md),    [SLA 99,9%](../../active-directory/fundamentals/active-directory-whatis.md) |  [Adições de gestão/aplicação de self-service/self-service/Grupos dinâmicos](../../active-directory/fundamentals/active-directory-whatis.md),   [Redefinição/Alteração/Desbloqueio com write-back no local,](../../active-directory/fundamentals/active-directory-whatis.md)    [Autenticação multi-factor (Cloud and On-in(MFA Server))](../../active-directory/fundamentals/active-directory-whatis.md), [MIM CAL + SERVIDOR MIM,](../../active-directory/fundamentals/active-directory-whatis.md)Cloud App     [Discovery](../../active-directory/fundamentals/active-directory-whatis.md),  [Connect Health](../../active-directory/fundamentals/active-directory-whatis.md),   [Rollover automático de passwords para contas de grupo](../../active-directory/fundamentals/active-directory-whatis.md)|   [Proteção de Identidade](../../active-directory/identity-protection/overview-identity-protection.md),  [Gestão de Identidade Privilegiada](../../active-directory/privileged-identity-management/pim-configure.md)|   [Junte um dispositivo a Azure AD, Desktop SSO, Microsoft Passport for Azure AD, Administrator BitLocker recovery](../../active-directory/fundamentals/active-directory-whatis.md),    [MDM auto-matricula, Self-Service recuperação bitLocker, administradores locais adicionais para dispositivos Windows 10 via Azure AD Join](../../active-directory/fundamentals/active-directory-whatis.md)|

- [Cloud App Discovery](/cloud-app-security/set-up-cloud-discovery) é uma funcionalidade premium do Azure Ative Directory que lhe permite identificar aplicações em nuvem que são usadas pelos colaboradores da sua organização.

- [A Azure Ative Directory Identity Protection](../../active-directory/identity-protection/overview-identity-protection.md) é um serviço de segurança que utiliza capacidades de deteção de anomalias do Azure Ative Directory para fornecer uma visão consolidada de deteções de riscos e potenciais vulnerabilidades que podem afetar as identidades da sua organização.

- [Os Serviços de Domínio do Diretório Ativo Azure](https://azure.microsoft.com/services/active-directory-ds/) permitem-lhe juntar VMs Azure a um domínio sem a necessidade de implantar controladores de domínio. Os utilizadores inscrevem-se nestes VMs utilizando as suas credenciais de Diretório Ativo corporativo, e podem aceder perfeitamente aos recursos.

- [O Azure Ative Directory B2C](https://azure.microsoft.com/services/active-directory-b2c/) é um serviço de gestão de identidade global e altamente disponível para aplicações viradas para o consumidor que podem escalar para centenas de milhões de identidades e integrar-se em plataformas móveis e web. Os seus clientes podem iniciar sessão em todas as suas apps através de experiências personalizáveis que utilizam as contas existentes nas redes sociais, ou pode criar novas credenciais autónomas.

- [A Azure Ative Directory B2B Collaboration](../../active-directory/external-identities/what-is-b2b.md) é uma solução segura de integração de parceiros que suporta as suas relações intercomuárias, permitindo aos parceiros acederem seletivamente às suas aplicações corporativas e aos seus dados utilizando as suas identidades auto-geridas.

- [O Azure Ative Directory aderiu](../../active-directory/devices/overview.md) permite-lhe alargar as capacidades da cloud aos dispositivos Windows 10 para gestão centralizada. Permite que os utilizadores se conectem à nuvem corporativa ou organizacional através do Azure Ative Directory e simplifica o acesso a apps e recursos.

- [O Azure Ative Directory Application Proxy](../../active-directory/manage-apps/application-proxy.md) fornece SSO e acesso remoto seguro para aplicações web hospedadas no local.

## <a name="next-steps"></a>Passos Seguintes

- Compreenda a sua [responsabilidade partilhada na nuvem.](shared-responsibility.md)

- Saiba como o [Azure Security Center](../../security-center/security-center-introduction.md) pode ajudá-lo a prevenir, detetar e responder a ameaças com maior visibilidade e controlo sobre a segurança dos seus recursos Azure.