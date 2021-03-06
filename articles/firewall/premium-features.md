---
title: Funcionalidades de pré-visualização Azure Firewall Premium
description: O Azure Firewall Premium é um serviço de segurança de rede gerido e baseado na nuvem que protege os seus recursos da Rede Virtual Azure.
author: vhorne
ms.service: firewall
services: firewall
ms.topic: conceptual
ms.date: 04/07/2021
ms.author: victorh
ms.custom: references_regions
ms.openlocfilehash: fdf0113d73b0b2e82ec889caf2a82c77bb7a040c
ms.sourcegitcommit: b28e9f4d34abcb6f5ccbf112206926d5434bd0da
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/09/2021
ms.locfileid: "107226706"
---
# <a name="azure-firewall-premium-preview-features"></a>Funcionalidades de pré-visualização Azure Firewall Premium

Logotipo :::image type="content" source="media/premium-features/icsa-cert-firewall-small.png" alt-text="de certificação ICSA Logotipo" border="false"::::::image type="content" source="media/premium-features/pci-logo.png" alt-text="de certificação PCI" border="false":::


> [!IMPORTANT]
> O Azure Firewall Premium está atualmente em pré-visualização pública.
> Esta versão de pré-visualização é disponibiliza sem um contrato de nível de serviço e não é recomendada para cargas de trabalho de produção. Algumas funcionalidades poderão não ser suportadas ou poderão ter capacidades limitadas. Para obter mais informações, veja [Termos Suplementares de Utilização para Pré-visualizações do Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

 A Azure Firewall Premium Preview é uma firewall de próxima geração com capacidades que são necessárias para ambientes altamente sensíveis e regulados.

:::image type="content" source="media/premium-features/premium-overview.png" alt-text="Diagrama de visão geral do Azure Firewall Premium":::

A Azure Firewall Premium Preview utiliza firewall Policy, um recurso global que pode ser usado para gerir centralmente as suas firewalls usando o Azure Firewall Manager. A partir desta versão, todas as novas funcionalidades são configurais apenas através da Política de Firewall. As regras de firewall (clássicas) continuam a ser suportadas e podem ser usadas para configurar as funcionalidades normais existentes de Firewall.  A Política de Firewall pode ser gerida de forma independente ou com o Azure Firewall Manager. Uma política de firewall associada a uma única firewall não tem qualquer custo.

> [!IMPORTANT]
> Atualmente, o Firewall Premium SKU não é suportado em implementações de Secure Hub e configurações de túneis forçados. 

A pré-visualização Azure Firewall Premium inclui as seguintes funcionalidades:

- **Inspeção TLS** - desencripta o tráfego de saída, processa os dados, em seguida, encripta os dados e envia-os para o destino.
- **IDPS** - Um sistema de deteção e prevenção de intrusões em rede (IDPS) permite monitorizar atividades de rede para atividades maliciosas, registar informações sobre esta atividade, denunciá-la e tentar bloqueá-la opcionalmente.
- **Filtragem de URL** - estende a capacidade de filtragem FQDN da Azure Firewall para considerar um URL inteiro. Por exemplo, `www.contoso.com/a/c` em vez `www.contoso.com` de. .
- **Categorias web** - os administradores podem permitir ou negar o acesso do utilizador a categorias de websites, tais como sites de jogos, sites de redes sociais, entre outros.


## <a name="tls-inspection"></a>Inspeção TLS

O Azure Firewall Premium termina as ligações TLS de saída e leste-oeste. A inspeção de entrada TLS é suportada com o Gateway de [aplicação Azure](../web-application-firewall/ag/ag-overview.md) permitindo encriptação de ponta a ponta. O Azure Firewall faz as funções de segurança de valor acrescentado necessárias e reencriptizou o tráfego que é enviado para o destino original.

> [!TIP]
> TLS 1.0 e 1.1 estão sendo depreciados e não serão apoiados. As versões TLS 1.0 e 1.1 da Camada de Tomadas TLS/Secure Sockets (SSL) foram consideradas vulneráveis e, embora ainda atualmente trabalhem para permitir retrocompatibilidade, não são recomendadas. Migrar para TLS 1.2 o mais rápido possível.

Para saber mais sobre os requisitos do certificado de pré-visualização intermédia de pré-visualização Azure Firewall Premium, consulte [os certificados de pré-visualização Azure Firewall Premium](premium-certificates.md).

## <a name="idps"></a>IDPS

Um sistema de deteção e prevenção de intrusões de rede (IDPS) permite-lhe monitorizar a sua rede para atividades maliciosas, registar informações sobre esta atividade, denunciá-la e tentar bloqueá-la opcionalmente. 

A Azure Firewall Premium Preview fornece IDPS baseado em assinaturas para permitir a deteção rápida de ataques procurando padrões específicos, tais como sequências de byte no tráfego de rede, ou sequências de instruções maliciosas conhecidas usadas por malware. As assinaturas IDPS são totalmente geridas e continuamente atualizadas.

As assinaturas/regras do Azure Firewall incluem:
- Uma ênfase na recolha de impressões digitais de malware real, Comando e Controlo, explorar kits e na atividade maliciosa selvagem perdida pelos métodos tradicionais de prevenção.
- Mais de 35.000 regras em mais de 50 categorias.
    - As categorias incluem comando e controlo de malware, ataques DoS, botnets, eventos informativos, explorações, vulnerabilidades, protocolos de rede SCADA, atividade de kit de exploração, e muito mais.
- Todas as manhãs são lançadas mais de 20 a 40 novas regras.
- Baixa classificação positiva falsa usando a caixa de areia de malware de última geração e o ciclo de feedback da rede de sensores global.

O IDPS permite-lhe detetar ataques em todas as portas e protocolos para tráfego não encriptado. No entanto, quando o tráfego HTTPS precisa de ser inspecionado, o Azure Firewall pode usar a sua capacidade de inspeção TLS para desencriptar o tráfego e detetar melhor as atividades maliciosas.  

A Lista de Bypass IDPS permite-lhe não filtrar o tráfego para nenhum dos endereços, gamas e sub-redes IP especificados na lista de bypass. 

## <a name="url-filtering"></a>Filtragem de URL

A filtragem de URL estende a capacidade de filtragem FQDN da Azure Firewall para considerar um URL inteiro. Por exemplo, `www.contoso.com/a/c` em vez `www.contoso.com` de. .  

A filtragem de URL pode ser aplicada tanto no tráfego HTTP como HTTPS. Quando o tráfego HTTPS é inspecionado, a Pré-visualização Azure Firewall Premium pode utilizar a sua capacidade de inspeção TLS para desencriptar o tráfego e extrair o URL-alvo para validar se o acesso é permitido. A inspeção TLS requer opt-in ao nível da regra de aplicação. Uma vez ativados, pode utilizar URLs para filtrar com HTTPS. 

## <a name="web-categories"></a>Categorias web

As categorias web permitem que os administradores permitam ou neguem o acesso dos utilizadores a categorias de sites de jogos, sites de jogos de azar, sites de redes sociais e outros. As categorias web também serão incluídas no Azure Firewall Standard, mas serão mais afinadas na Pré-visualização Azure Firewall Premium. Ao contrário da capacidade das categorias Web no SKU Standard que corresponde à categoria baseada numa FQDN, o Premium SKU corresponde à categoria de acordo com todo o URL para o tráfego HTTP e HTTPS. 

Por exemplo, se o Azure Firewall intercetar um pedido HTTPS `www.google.com/news` para, espera-se a seguinte categorização: 

- Firewall Standard – apenas a parte FQDN será examinada, pelo `www.google.com` que será classificada como Motor de *Busca.* 

- Firewall Premium – o URL completo será examinado, pelo `www.google.com/news` que será classificado como *Notícias.*

As categorias são organizadas com base na gravidade em **Responsabilidade,** **Largura de Banda Alta,** **Uso de Negócios,** Perda **de Produtividade,** **Surf Geral** e **Uncategorized.**

### <a name="category-exceptions"></a>Exceções de categorias

Pode criar exceções às regras da sua categoria web. Crie uma recolha de regras de permitir ou negar regras separadamente com uma prioridade maior dentro do grupo de recolha de regras. Por exemplo, pode configurar uma coleção de regras que permite `www.linkedin.com` com prioridade 100, com uma coleção de regras que nega **a rede social** com prioridade 200. Isto cria a exceção para a categoria web de **redes sociais** pré-definida.

### <a name="categorization-change"></a>Alteração da categorização

Pode solicitar uma alteração de categorização se:

 - acho que um FQDN ou URL deve estar sob uma categoria diferente 
 
ou 

- têm uma categoria sugerida para um FQDN não categorizado ou URL

Pode apresentar um pedido [https://aka.ms/azfw-webcategories-request](https://aka.ms/azfw-webcategories-request) em.
 
## <a name="supported-regions"></a>Regiões suportadas

A pré-visualização Azure Firewall Premium é suportada nas seguintes regiões:

- Austrália Central (Público / Austrália)
- Austrália Central 2 (Público / Austrália)
- Austrália Oriental (Público / Austrália)
- Sudeste da Austrália (Público / Austrália)
- Brasil Sul (Público / Brasil)
- Canadá Central (Público / Canadá)
- Canadá Leste (Público / Canadá)
- Central dos EUA (Público / Estados Unidos)
- Eua EUA (Público/ Canário (EUA))
- Ásia Oriental (Público / Ásia Pacífico)
- Leste dos EUA (Público / Estados Unidos)
- Leste dos EUA 2 (Público / Estados Unidos)
- France Central (Público / França)
- França Sul (Público / França)
- Japão Oriental (Público / Japão)
- Japão Ocidental (Público / Japão)
- Coreia Central (Público / Coreia)
- Coreia do Sul (Público / Coreia)
- Norte Central DOS EUA (Público / Estados Unidos)
- Norte da Europa (Público / Europa)
- África do Sul Norte (África do Sul/ África do Sul)
- South Central US (Público / Estados Unidos)
- Sudeste Asiático (Público / Ásia Pacífico)
- UAE Central (Público/ EAU)
- Reino Unido Sul (Público / Reino Unido)
- UK West (Público / Reino Unido)
- West Central US (Público / Estados Unidos)
- Europa Ocidental (Público / Europa)
- Índia Ocidental (Público / Índia)
- West US (Público / Estados Unidos)
- West US 2 (Público / Estados Unidos)


## <a name="known-issues"></a>Problemas conhecidos

A Azure Firewall Premium Preview tem os seguintes problemas conhecidos:

|Problema  |Description  |Mitigação  |
|---------|---------|---------|
|Inspeção TLS suportada apenas na porta padrão HTTPS|A Inspeção TLS suporta apenas HTTPS/443. |Nenhum. Outros portos serão apoiados em GA.|
|Apoio ESNI à resolução FQDN em HTTPS|O SNI encriptado não é suportado no aperto de mão HTTPS.|Hoje apenas o Firefox suporta o ESNI através da configuração personalizada. A solução proposta é para desativar esta funcionalidade.|
|Certificados de Cliente (TLS)|Os certificados de cliente são usados para construir uma confiança de identidade mútua entre o cliente e o servidor. Os certificados de cliente são usados durante uma negociação TLS. A azure firewall renegocia uma ligação com o servidor e não tem acesso à chave privada dos certificados do cliente.|Nenhum|
|QUIC/HTTP3|QUIC é a nova versão principal do HTTP. É um protocolo baseado na UDP com mais de 80 (PLAN) e 443 (SSL). A inspeção FQDN/URL/TLS não será suportada.|Configure a passagem do UDP 80/443 como regras de rede.|
Certificados assinados por clientes não fidedquis para clientes|Os certificados assinados pelo cliente não são fidedignos pela firewall uma vez recebidos de um servidor web baseado em intranet.|Correção marcada para a AG.
|Endereços IP de origem e destino errados em Alertas para IDPS com inspeção TLS.|Quando ativa a inspeção TLS e o IDPS emite um novo alerta, o endereço IP de origem/destino apresentado está errado (o endereço IP interno é apresentado em vez do endereço IP original).|Correção marcada para a AG.|
|Endereço IP de origem errada em Alertas com IDPS para HTTP (sem inspeção TLS).|Quando o tráfego HTTP de texto simples está em uso, e iDPS emite um novo alerta, e o destino é público um endereço IP, o endereço IP de origem visualizado está errado (o endereço IP interno é apresentado em vez do endereço IP original).|Correção marcada para a AG.|
|Propagação de Certificados|Após a aplicação de um certificado de CA na firewall, pode demorar entre 5 a 10 minutos para o certificado entrar em vigor.|Correção marcada para a AG.|
|IDPS Bypass|O Bypass IDPS não funciona para o tráfego terminado TLS, e o endereço IP de origem e os grupos IP source não são suportados.|Correção marcada para a AG.|
|Suporte TLS 1.3|TLS 1.3 é parcialmente suportado. O túnel TLS do cliente para a firewall baseia-se no TLS 1.2, e da firewall para o servidor web externo é baseado em TLS 1.3.|As atualizações estão a ser investigadas.|
|KeyVault Private Endpoint|O KeyVault suporta o acesso private Endpoint para limitar a sua exposição à rede. Os Serviços de Azure Fidedigno podem contornar esta limitação se uma exceção for configurada como descrito na [documentação KeyVault](../key-vault/general/overview-vnet-service-endpoints.md#trusted-services). A Azure Firewall não está atualmente listada como um serviço de confiança e não pode aceder ao Cofre de Chaves.|Correção marcada para a AG.|


## <a name="next-steps"></a>Passos seguintes

- [Saiba mais sobre os certificados Azure Firewall Premium](premium-certificates.md)
- [Implementar e configurar a pré-visualização Azure Firewall Premium](premium-deploy.md)
- [Migrar para Azure Firewall Premium Preview](premium-migrate.md)
