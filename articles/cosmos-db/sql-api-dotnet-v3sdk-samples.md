---
title: 'Azure Cosmos DB: .NET (Microsoft.Azure.Cosmos) exemplos para o SQL API'
description: Encontre os exemplos C# .NET V3 SDK no GitHub para tarefas comuns utilizando a API AZure Cosmos DB SQL.
author: SnehaGunda
ms.service: cosmos-db
ms.subservice: cosmosdb-sql
ms.topic: sample
ms.date: 10/07/2019
ms.author: sngun
ms.custom: devx-track-dotnet
ms.openlocfilehash: 30ccd5783b3fa9524072235a58b3c8708962ef1c
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/20/2021
ms.locfileid: "102434020"
---
# <a name="azure-cosmos-dbnet-v3-sdk-microsoftazurecosmos-examples-for-the-sql-api"></a>Azure Cosmos DB.NET V3 SDK (Microsoft.Azure.Cosmos) para o SQL API
[!INCLUDE[appliesto-sql-api](includes/appliesto-sql-api.md)]

> [!div class="op_single_selector"]
> * [.NET V2 SDK Exemplos](sql-api-dotnet-samples.md)
> * [.NET V3 SDK Exemplos](sql-api-dotnet-v3sdk-samples.md)
> * [Exemplos java V4 SDK](sql-api-java-sdk-samples.md)
> * [Dados de primavera V3 Exemplos SDK](sql-api-spring-data-sdk-samples.md)
> * [Exemplos de Node.js](sql-api-nodejs-samples.md)
> * [Exemplos de Python](sql-api-python-samples.md)
> * [Galeria de Exemplo de Código do Azure](https://azure.microsoft.com/resources/samples/?sort=0&service=cosmos-db)
>
>

O [repositório azure-cosmos-dotnet-v3](https://github.com/Azure/azure-cosmos-dotnet-v3/tree/master/Microsoft.Azure.Cosmos.Samples/Usage) GitHub inclui as mais recentes soluções de amostra .NET para executar CRUD e outras operações comuns em recursos DB da Azure Cosmos. Se estiver familiarizado com a versão anterior do .NET SDK, poderá estar habituado à recolha e documento dos termos. Como a Azure Cosmos DB suporta vários modelos API, a versão 3.0 do .NET SDK utiliza os termos genéricos "recipiente" e "item". Os contentores podem ser uma coleção, um grafo ou uma tabela. Os itens podem ser um documento, um vértice/aresta ou uma linha e são o conteúdo dentro dos contentores. Este artigo fornece:

* Ligações para as tarefas em cada um dos ficheiros do projeto de C# de exemplo.
* Ligações para o conteúdo de referência da API relacionada.

## <a name="prerequisites"></a>Pré-requisitos

Visual Studio 2019 com o fluxo de trabalho de desenvolvimento Azure instalado

- Você pode baixar  e usar a [Edição Comunitária visual 2019 gratuita do Visual Studio 2019.](https://www.visualstudio.com/downloads/) Confirme que ativa o **desenvolvimento do Azure** durante a configuração do Visual Studio.

   O [pacote Microsoft.Azure.cosmos NuGet](https://www.nuget.org/packages/Microsoft.Azure.cosmos/)

Uma subscrição Azure ou conta de testes gratuitos da Cosmos DB

- [!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]
  
- Pode [ativar os benefícios do assinante do Visual Studio](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio): A subscrição do Visual Studio dá-lhe créditos mensais, que pode utilizar para serviços Azure pagos.
- [!INCLUDE [cosmos-db-emulator-docdb-api](../../includes/cosmos-db-emulator-docdb-api.md)]  

> [!NOTE]
> As amostras são autossuficientes, e configuradas e limpas depois de si mesmas. Cada ocorrência fatura a sua subscrição por uma hora de utilização no nível de desempenho do seu contentor.
> 

## <a name="database-examples"></a>Exemplos de base de dados

O método [RunDatabaseDemo](https://github.com/Azure/azure-cosmos-dotnet-v3/blob/master/Microsoft.Azure.Cosmos.Samples/Usage/DatabaseManagement/Program.cs#L65-L91) do projeto *Sample DatabaseManagement* mostra como fazer as seguintes tarefas. Para saber mais sobre as bases de dados da Azure Cosmos antes de executar as seguintes amostras, consulte [Work with databases, containers e itens](account-databases-containers-items.md).

| Tarefa | Referência da API |
| --- | --- |
| [Criar uma base de dados](https://github.com/Azure/azure-cosmos-dotnet-v3/blob/master/Microsoft.Azure.Cosmos.Samples/Usage/DatabaseManagement/Program.cs#L68) |[CosmosClient.CreateDatabaseIfNotExistsAsync](/dotnet/api/microsoft.azure.cosmos.cosmosclient.createdatabaseifnotexistsasync) |
| [Leia uma base de dados por ID](https://github.com/Azure/azure-cosmos-dotnet-v3/blob/master/Microsoft.Azure.Cosmos.Samples/Usage/DatabaseManagement/Program.cs#L80) |[Base de dados.ReadAsync](/dotnet/api/microsoft.azure.cosmos.database.readasync) |
| [Leia todas as bases de dados para uma conta](https://github.com/Azure/azure-cosmos-dotnet-v3/blob/master/Microsoft.Azure.Cosmos.Samples/Usage/DatabaseManagement/Program.cs#L96) |[CosmosClient.GetDatabaseQueryIterator](/dotnet/api/microsoft.azure.cosmos.cosmosclient.getdatabasequeryiterator) |
| [Eliminar uma base de dados](https://github.com/Azure/azure-cosmos-dotnet-v3/blob/master/Microsoft.Azure.Cosmos.Samples/Usage/DatabaseManagement/Program.cs#L106) |[Base de dados.DeleteAsync](/dotnet/api/microsoft.azure.cosmos.database.deleteasync) |

## <a name="container-examples"></a>Exemplos de contentor

O método [RunContainerDemo](https://github.com/Azure/azure-cosmos-dotnet-v3/blob/master/Microsoft.Azure.Cosmos.Samples/Usage/ContainerManagement/Program.cs#L69-L89) do projeto *Demanagement de amostra* mostra como fazer as seguintes tarefas. Para saber mais sobre os recipientes Azure Cosmos antes de executar as seguintes amostras, consulte [Work with databases, containers e itens](account-databases-containers-items.md).

| Tarefa | Referência da API |
| --- | --- |
| [Criar um contentor](https://github.com/Azure/azure-cosmos-dotnet-v3/blob/master/Microsoft.Azure.Cosmos.Samples/Usage/ContainerManagement/Program.cs#L97-L107) |[Base de dados.CreateContainerIfNotExistsAsync](/dotnet/api/microsoft.azure.cosmos.database.createcontainerifnotexistsasync) |
| [Criar um recipiente com política de índice personalizado](https://github.com/Azure/azure-cosmos-dotnet-v3/blob/master/Microsoft.Azure.Cosmos.Samples/Usage/ContainerManagement/Program.cs#L161-L178) |[Base de dados.CreateContainerIfNotExistsAsync](/dotnet/api/microsoft.azure.cosmos.database.createcontainerifnotexistsasync) |
| [Alterar desempenho configurado de um recipiente](https://github.com/Azure/azure-cosmos-dotnet-v3/blob/master/Microsoft.Azure.Cosmos.Samples/Usage/ContainerManagement/Program.cs#L198-L223) |[Contentor.ReplaceThroughputAsync](/dotnet/api/microsoft.azure.cosmos.container.replacethroughputasync) |
| [Obtenha um recipiente por ID](https://github.com/Azure/azure-cosmos-dotnet-v3/blob/master/Microsoft.Azure.Cosmos.Samples/Usage/ContainerManagement/Program.cs#L225-L236) |[Contentor.ReadContainerAsync](/dotnet/api/microsoft.azure.cosmos.container.readcontainerasync) |
| [Leia todos os recipientes numa base de dados](https://github.com/Azure/azure-cosmos-dotnet-v3/blob/master/Microsoft.Azure.Cosmos.Samples/Usage/ContainerManagement/Program.cs#L242-L258) |[Base de dados.GetContainerQueryIterator](/dotnet/api/microsoft.azure.cosmos.database.getcontainerqueryiterator) |
| [Eliminar um contentor](https://github.com/Azure/azure-cosmos-dotnet-v3/blob/master/Microsoft.Azure.Cosmos.Samples/Usage/ContainerManagement/Program.cs#L264-L270) |[Contentor.DeleteContainerAsync](/dotnet/api/microsoft.azure.cosmos.container.deletecontainerasync) |

## <a name="item-examples"></a>Exemplos de itens

O método [RunItemsDemo](https://github.com/Azure/azure-cosmos-dotnet-v3/blob/master/Microsoft.Azure.Cosmos.Samples/Usage/ItemManagement/Program.cs#L119-L130) do projeto *ItemManagement* da amostra mostra como fazer as seguintes tarefas. Para saber mais sobre os itens da Azure Cosmos antes de executar as seguintes amostras, consulte [Work with databases, containers e itens](account-databases-containers-items.md).

| Tarefa | Referência da API |
| --- | --- |
| [Criar um item](https://github.com/Azure/azure-cosmos-dotnet-v3/blob/master/Microsoft.Azure.Cosmos.Samples/Usage/ItemManagement/Program.cs#L161-L200) |[Contentor.CreateItemAsync](/dotnet/api/microsoft.azure.cosmos.container.createitemasync) |
| [Ler um item por ID](https://github.com/Azure/azure-cosmos-dotnet-v3/blob/master/Microsoft.Azure.Cosmos.Samples/Usage/ItemManagement/Program.cs#L203-L241) |[contentor. ReadItemAsync](/dotnet/api/microsoft.azure.cosmos.container.readitemasync) |
| [Consulta de itens](https://github.com/Azure/azure-cosmos-dotnet-v3/blob/master/Microsoft.Azure.Cosmos.Samples/Usage/ItemManagement/Program.cs#L244-L320) |[contentor. GetItemQueryIterator](/dotnet/api/microsoft.azure.cosmos.container.getitemqueryiterator) |
| [Substituir um item](https://github.com/Azure/azure-cosmos-dotnet-v3/blob/master/Microsoft.Azure.Cosmos.Samples/Usage/ItemManagement/Program.cs#L415-L456) |[contentor. SubstituItemAsync](/dotnet/api/microsoft.azure.cosmos.container.replaceitemasync) |
| [Aumentar um item](https://github.com/Azure/azure-cosmos-dotnet-v3/blob/master/Microsoft.Azure.Cosmos.Samples/Usage/ItemManagement/Program.cs#L458-L509) |[contentor. UpsertItemAsync](/dotnet/api/microsoft.azure.cosmos.container.upsertitemasync) |
| [Eliminar um item](https://github.com/Azure/azure-cosmos-dotnet-v3/blob/master/Microsoft.Azure.Cosmos.Samples/Usage/ItemManagement/Program.cs#L511-L522) |[contentor. DeleteItemAsync](/dotnet/api/microsoft.azure.cosmos.container.deleteitemasync) |
| [Substitua um item por verificação ETag condicional](https://github.com/Azure/azure-cosmos-dotnet-v3/blob/master/Microsoft.Azure.Cosmos.Samples/Usage/ItemManagement/Program.cs#L682-L772) |[Opções de Pedidos.IfMatchEtag](/dotnet/api/microsoft.azure.cosmos.requestoptions.ifmatchetag) |

## <a name="indexing-examples"></a>Exemplos de indexação

O método [RunIndexDemo](https://github.com/Azure/azure-cosmos-dotnet-v3/blob/master/Microsoft.Azure.Cosmos.Samples/Usage/IndexManagement/Program.cs#L108-L122) do projeto *IndexManagement* da amostra mostra como fazer as seguintes tarefas. Para saber mais sobre a indexação em Azure Cosmos DB antes de executar as seguintes amostras, consulte as políticas de [índice,](index-policy.md) [os tipos de índices](index-overview.md#index-types)e [os caminhos de índice](index-policy.md#include-exclude-paths). 

| Tarefa | Referência da API |
| --- | --- |
| [Excluir um item do índice](https://github.com/Azure/azure-cosmos-dotnet-v3/blob/master/Microsoft.Azure.Cosmos.Samples/Usage/IndexManagement/Program.cs#L130-L186) |[IndexingDirective.Exclude](/dotnet/api/microsoft.azure.cosmos.indexingdirective) |
| [Use indexação preguiçosa](https://github.com/Azure/azure-cosmos-dotnet-v3/blob/master/Microsoft.Azure.Cosmos.Samples/Usage/IndexManagement/Program.cs#L198-L220) |[IndexingPolicy.IndexingMode](/dotnet/api/microsoft.azure.cosmos.indexingpolicy.indexingmode) |
| [Excluir caminhos de item especificados do índice](https://github.com/Azure/azure-cosmos-dotnet-v3/blob/master/Microsoft.Azure.Cosmos.Samples/Usage/IndexManagement/Program.cs#L230-L297) |[IndexingPolicy.ExcludedPaths](/dotnet/api/microsoft.azure.cosmos.indexingpolicy.excludedpaths) |

## <a name="query-examples"></a>Exemplos de consultas

O método [RunDemoAsync](https://github.com/Azure/azure-cosmos-dotnet-v3/blob/master/Microsoft.Azure.Cosmos.Samples/Usage/Queries/Program.cs#L76-L96) do projeto *de consultas* de amostra mostra como fazer as seguintes tarefas usando a gramática de consulta SQL, o provedor LINQ com consulta, e Lambda. Para saber mais sobre a referência de consulta SQL em Azure Cosmos DB antes de executar as [seguintes amostras, consulte exemplos de consulta SQL para Azure Cosmos DB](./sql-query-getting-started.md).

| Tarefa | Referência da API |
| --- | --- |
| [Artigos de consulta a partir de uma única partição](https://github.com/Azure/azure-cosmos-dotnet-v3/blob/master/Microsoft.Azure.Cosmos.Samples/Usage/Queries/Program.cs#L154-L186) |[contentor. GetItemQueryIterator](/dotnet/api/microsoft.azure.cosmos.container.getitemqueryiterator) |
| [Artigos de consulta de múltiplas divisórias](https://github.com/Azure/azure-cosmos-dotnet-v3/blob/master/Microsoft.Azure.Cosmos.Samples/Usage/Queries/Program.cs#L215-L275) |[contentor. GetItemQueryIterator](/dotnet/api/microsoft.azure.cosmos.container.getitemqueryiterator) |
| [Consulta usando uma declaração SQL](https://github.com/Azure/azure-cosmos-dotnet-v3/blob/master/Microsoft.Azure.Cosmos.Samples/Usage/Queries/Program.cs#L189-L212) |[contentor. GetItemQueryIterator](/dotnet/api/microsoft.azure.cosmos.container.getitemqueryiterator) |

## <a name="change-feed-examples"></a>Exemplos de feed de alterações

O método [RunBasicChangeFeed](https://github.com/Azure/azure-cosmos-dotnet-v3/blob/master/Microsoft.Azure.Cosmos.Samples/Usage/ChangeFeed/Program.cs#L91-L119) do projeto *ChangeFeed* da amostra mostra como fazer as seguintes tarefas. Para saber mais sobre a mudança de feed em Azure Cosmos DB antes de executar as seguintes amostras, consulte [ler Azure Cosmos DB alterar feed feed](read-change-feed.md) e alterar processador de [feed](change-feed-processor.md).

| Tarefa | Referência da API |
| --- | --- |
| [Funcionalidade básica de feed de alteração](https://github.com/Azure/azure-cosmos-dotnet-v3/blob/master/Microsoft.Azure.Cosmos.Samples/Usage/ChangeFeed/Program.cs#L91-L119) |[Container.GetChangeFeedProcessorBuilder](/dotnet/api/microsoft.azure.cosmos.container.getchangefeedprocessorbuilder) |
| [Leia o feed de mudança a partir de um momento específico](https://github.com/Azure/azure-cosmos-dotnet-v3/blob/master/Microsoft.Azure.Cosmos.Samples/Usage/ChangeFeed/Program.cs#L127-L162) |[Container.GetChangeFeedProcessorBuilder](/dotnet/api/microsoft.azure.cosmos.container.getchangefeedprocessorbuilder) |
| [Leia o feed de mudança desde o início](https://github.com/Azure/azure-cosmos-dotnet-v3/blob/master/Microsoft.Azure.Cosmos.Samples/Usage/ChangeFeed/Program.cs#L170-L198) |[ChangeFeedProcessorBuilder.WithStartTime (DateTime)](/dotnet/api/microsoft.azure.cosmos.changefeedprocessorbuilder.withstarttime) |
| [MIgrate do processador de feed de alteração para alterar feed em V3 SDK](https://github.com/Azure/azure-cosmos-dotnet-v3/blob/master/Microsoft.Azure.Cosmos.Samples/Usage/ChangeFeed/Program.cs#L256-L333) |[Container.GetChangeFeedProcessorBuilder](/dotnet/api/microsoft.azure.cosmos.container.getchangefeedprocessorbuilder) |

## <a name="server-side-programming-examples"></a>Exemplos de programação do lado do servidor

O método [RunDemoAsync](https://github.com/Azure/azure-cosmos-dotnet-v3/blob/master/Microsoft.Azure.Cosmos.Samples/Usage/ServerSideScripts/Program.cs#L72-L102) do projeto *ServerSideScripts* da amostra mostra como fazer as seguintes tarefas. Para saber mais sobre a programação do lado do servidor no Azure Cosmos DB antes de executar as [seguintes amostras, consulte procedimentos armazenados, gatilhos e funções definidas pelo utilizador](stored-procedures-triggers-udfs.md).

| Tarefa | Referência da API |
| --- | --- |
| [Criar um procedimento armazenado](https://github.com/Azure/azure-cosmos-dotnet-v3/blob/master/Microsoft.Azure.Cosmos.Samples/Usage/ServerSideScripts/Program.cs#L116) |[Scripts.CreateStoredProcedureAsync](/dotnet/api/microsoft.azure.cosmos.scripts.scripts.createstoredprocedureasync) |
| [Executar um procedimento armazenado](https://github.com/Azure/azure-cosmos-dotnet-v3/blob/master/Microsoft.Azure.Cosmos.Samples/Usage/ServerSideScripts/Program.cs#L135) |[Scripts.ExebonitoStoredProcedureAsync](/dotnet/api/microsoft.azure.cosmos.scripts.scripts.executestoredprocedureasync) |
| [Eliminar um procedimento armazenado](https://github.com/Azure/azure-cosmos-dotnet-v3/blob/master/Microsoft.Azure.Cosmos.Samples/Usage/ServerSideScripts/Program.cs#L351) |[Scripts.DeleteStoredProcedureAsync](/dotnet/api/microsoft.azure.cosmos.scripts.scripts.deletestoredprocedureasync) |
