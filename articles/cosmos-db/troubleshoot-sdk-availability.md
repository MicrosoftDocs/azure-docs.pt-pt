---
title: Diagnosticar e resolver problemas da disponibilidade de Azure Cosmos SDKs em ambientes multi-regionais
description: Saiba tudo sobre o comportamento de disponibilidade do Azure Cosmos SDK ao operar em ambientes multi-regionais.
author: ealsur
ms.service: cosmos-db
ms.date: 02/18/2021
ms.author: maquaran
ms.subservice: cosmosdb-sql
ms.topic: troubleshooting
ms.reviewer: sngun
ms.openlocfilehash: 0720eb01920e39a9bee27e4d00d97acba55b0ad5
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/29/2021
ms.locfileid: "101661431"
---
# <a name="diagnose-and-troubleshoot-the-availability-of-azure-cosmos-sdks-in-multiregional-environments"></a>Diagnosticar e resolver problemas da disponibilidade de Azure Cosmos SDKs em ambientes multi-regionais
[!INCLUDE[appliesto-sql-api](includes/appliesto-sql-api.md)]

Este artigo descreve o comportamento da versão mais recente dos SDKs Azure Cosmos quando você vê um problema de conectividade para uma determinada região ou quando ocorre uma falha de região.

Todos os SDKs Azure Cosmos dão-lhe uma opção para personalizar a preferência regional. As seguintes propriedades são utilizadas em diferentes SDKs:

* A [propriedade ConnectionPolicy.PreferredLocations](/dotnet/api/microsoft.azure.documents.client.connectionpolicy.preferredlocations) em .NET V2 SDK.
* [As Opções CosmosClient.ApplicationRegion](/dotnet/api/microsoft.azure.cosmos.cosmosclientoptions.applicationregion) ou [CosmosClientOptions.ApplicationPreferredRegions](/dotnet/api/microsoft.azure.cosmos.cosmosclientoptions.applicationpreferredregions) propriedades em .NET V3 SDK.
* O [método CosmosClientBuilder.preferredRegions](/java/api/com.azure.cosmos.cosmosclientbuilder.preferredregions) em Java V4 SDK.
* O [CosmosClient.preferred_locations](/python/api/azure-cosmos/azure.cosmos.cosmos_client.cosmosclient) parâmetro em Python SDK.
* O [parâmetro CosmosClientOptions.ConnectionPolicy.preferredLocations](/javascript/api/@azure/cosmos/connectionpolicy#preferredlocations) em JS SDK.

Quando definir a preferência regional, o cliente ligar-se-á a uma região como mencionado no quadro seguinte:

|Tipo de conta |Leituras |Escritas |
|------------------------|--|--|
| Região de escrita única | Região preferida | Região primária  |
| Múltiplas regiões de escrita | Região preferida | Região preferida  |

Se **não definir uma região preferida,** o cliente SDK falha na região primária:

|Tipo de conta |Leituras |Escritas |
|------------------------|--|--|
| Região de escrita única | Região primária | Região primária |
| Múltiplas regiões de escrita | Região primária  | Região primária  |

> [!NOTE]
> A região primária refere-se à primeira região da lista da região da [conta Azure Cosmos.](distribute-data-globally.md)
> Se os valores especificados como preferência regional não corresponderem a quaisquer regiões azure existentes, serão ignorados. Se corresponderem a uma região existente, mas a conta não é replicada, então o cliente ligar-se-á à próxima região preferida que corresponda ou à região primária.

> [!WARNING]
> A lógica de failover e disponibilidade descrita neste documento pode ser desativada na configuração do cliente, o que não é aconselhado a menos que a aplicação do utilizador resolva os erros de disponibilidade em si. Isto pode ser conseguido através de:
>
> * Definição da [propriedade ConnectionPolicy.EnableEndpointRediscovery](/dotnet/api/microsoft.azure.documents.client.connectionpolicy.enableendpointdiscovery) em .NET V2 SDK para falso.
> * Definir a propriedade [CosmosClientOptions.LimitToEndpoint](/dotnet/api/microsoft.azure.cosmos.cosmosclientoptions.limittoendpoint) em .NET V3 SDK para verdade.
> * Definir o [método CosmosClientBuilder.endpointDiscoveryEnabled](/java/api/com.azure.cosmos.cosmosclientbuilder.endpointdiscoveryenabled) em Java V4 SDK para falso.
> * Definir o parâmetro [CosmosClient.enable_endpoint_discovery](/python/api/azure-cosmos/azure.cosmos.cosmos_client.cosmosclient) em Python SDK em falso.
> * Definir o [CosmosClientOptions.ConnectionPolicy.enableEndpointDiscovery](/javascript/api/@azure/cosmos/connectionpolicy#enableEndpointDiscovery) parameter in JS SDK to false.

Em circunstâncias normais, o cliente SDK ligará à região preferida (se for definida uma preferência regional) ou à região primária (se não for definida qualquer preferência), e as operações serão limitadas a essa região, a menos que ocorra qualquer um dos cenários abaixo.

Nestes casos, o cliente que utiliza o Azure Cosmos SDK expõe registos e inclui as informações de re-tentar como parte da informação de diagnóstico da **operação:**

* *O pedidoDiagnosticsString* propriedade sobre respostas em .NET V2 SDK.
* A propriedade *Diagnostics* sobre respostas e exceções em .NET V3 SDK.
* O método *getDiagnostics()* nas respostas e exceções no SDK Java V4.

Ao determinar a próxima região por ordem de preferência, o cliente SDK utilizará a lista da região de conta, priorizando as regiões preferenciais (se houver).

Para obter um pormenor exaustivo sobre as garantias de SLA durante estes eventos, consulte as [AEA para obter disponibilidade.](high-availability.md#slas-for-availability)

## <a name="removing-a-region-from-the-account"></a><a id="remove-region"></a>Remoção de uma região da conta

Quando remover uma região de uma conta Azure Cosmos, qualquer cliente SDK que utilize a conta detetará a remoção da região através de um código de resposta de backend. O cliente marca então o ponto final regional como indisponível. O cliente retrigia a operação atual e todas as operações futuras são encaminhadas permanentemente para a região seguinte por ordem de preferência. Caso a lista de preferências tivesse apenas uma entrada (ou estivesse vazia), mas a conta tem outras regiões disponíveis, irá encaminhar-se para a região seguinte na lista de contas.

## <a name="adding-a-region-to-an-account"></a>Adicionar uma região a uma conta

A cada 5 minutos, o cliente Azure Cosmos SDK lê a configuração da conta e atualiza as regiões que conhece.

Se remover uma região e posteriormente adicioná-la de volta à conta, se a região adicionada tiver uma ordem de preferência regional mais elevada na configuração SDK do que a região atual ligada, o SDK voltará a utilizar esta região permanentemente. Após a deteção da região adicional, todos os pedidos futuros são direcionados para ela.

Se configurar o cliente para ligar preferencialmente a uma região que a conta Azure Cosmos não tem, a região preferida é ignorada. Se adicionar essa região mais tarde, o cliente deteta-a e mudará permanentemente para aquela região.

## <a name="fail-over-the-write-region-in-a-single-write-region-account"></a><a id="manual-failover-single-region"></a>Falha sobre a região de escrita numa única conta de uma única região de escrita

Se iniciar uma falha na atual região de escrita, o próximo pedido de escrita falhará com uma resposta conhecida. Quando esta resposta for detetada, o cliente irá consultar a conta para aprender a nova região de escrita e procede à retrys the current operation e encaminhar permanentemente todas as futuras operações de escrita para a nova região.

## <a name="regional-outage"></a>Paralisação regional

Se a conta for uma única região de escrita e a paragem regional ocorrer durante uma operação de escrita, o comportamento é semelhante a um [failover manual](#manual-failover-single-region). Para pedidos de leitura ou várias contas de regiões de escrita, o comportamento é semelhante à [remoção de uma região.](#remove-region)

## <a name="session-consistency-guarantees"></a>Garantias de consistência da sessão

Ao utilizar a [consistência da sessão,](consistency-levels.md#guarantees-associated-with-consistency-levels)o cliente precisa de garantir que pode ler os seus próprios escritos. Nas contas da região de escrita única, onde a preferência da região de leitura é diferente da região de escrita, pode haver casos em que o utilizador emite uma escrita e ao fazer uma leitura de uma região local, a região local ainda não recebeu a replicação de dados (velocidade de restrição da luz). Nestes casos, o SDK deteta a falha específica na operação de leitura e recauchuta a leitura na região primária para garantir a consistência da sessão.

## <a name="transient-connectivity-issues-on-tcp-protocol"></a>Problemas de conectividade transitórios no protocolo TCP

Em cenários em que o cliente Azure Cosmos SDK está configurado para utilizar o protocolo TCP, para um dado pedido, pode haver situações em que as condições da rede estão temporariamente a afetar a comunicação com um determinado ponto final. Estas condições temporárias de rede podem surgir como intervalos de tempo TCP e erros de serviço indisponíveis (HTTP 503). O cliente tentará novamente o pedido localmente no mesmo ponto final durante alguns segundos antes de sair do erro.

Se o utilizador tiver configurado uma lista de regiões preferenciais com mais de uma região e a conta Azure Cosmos for várias regiões de escrita ou uma única região de escrita e a operação for um pedido de leitura, o cliente detetará a falha local, e tentará novamente essa operação na região seguinte a partir da lista de preferências.

## <a name="next-steps"></a>Passos seguintes

* Reveja as [Disponibilidades SLAs](high-availability.md#slas-for-availability).
* Utilize o mais recente [.NET SDK](sql-api-sdk-dotnet-standard.md)
* Use o mais recente [Java SDK](sql-api-sdk-java-v4.md)
* Use o mais recente [Python SDK](sql-api-sdk-python.md)
* Use o último [Nó SDK](sql-api-sdk-node.md)
