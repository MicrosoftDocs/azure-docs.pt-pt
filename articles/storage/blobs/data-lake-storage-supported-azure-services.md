---
title: Serviços Azure que suportam Azure Data Lake Storage Gen2 | Microsoft Docs
description: Saiba quais os serviços da Azure integrados com a Azure Data Lake Storage Gen2
author: normesta
ms.subservice: data-lake-storage-gen2
ms.service: storage
ms.topic: conceptual
ms.date: 02/17/2021
ms.author: normesta
ms.reviewer: stewu
ms.openlocfilehash: 36e1a8a288e1f9b2a8d65ab966b607b594d66f4e
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/29/2021
ms.locfileid: "100653606"
---
# <a name="azure-services-that-support-azure-data-lake-storage-gen2"></a>Serviços Azure que suportam Azure Data Lake Storage Gen2

Você pode usar os serviços Azure para ingerir dados, realizar análises e criar representações visuais. Este artigo fornece uma lista de serviços Azure suportados, divulga o seu nível de suporte, e fornece-lhe links para artigos que o ajudam a usar estes serviços com a Azure Data Lake Storage Gen2.

## <a name="supported-azure-services"></a>Serviços do Azure suportados

Esta tabela lista os serviços Azure que pode utilizar com a Azure Data Lake Storage Gen2. Os itens que aparecem nestas tabelas mudarão ao longo do tempo à medida que o suporte continua a expandir-se.

> [!NOTE]
> O nível de suporte refere-se apenas à forma como o serviço é suportado com a Data Lake Storage Gen 2.

|Serviço do Azure |Nível de suporte |Azure AD |Chave Partilhada| Artigos relacionados |
|---------------|-------------------|---|---|---|
|Azure Data Factory|Disponível em Geral|Yes|Yes|[Carregue os dados no Azure Data Lake Storage Gen2 com a Azure Data Factory](../../data-factory/load-azure-data-lake-storage-gen2.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json)|
|Azure Databricks|Disponível em Geral|Yes|Yes|[Utilizar com o Azure Databricks](/azure/databricks/data/data-sources/azure/azure-datalake-gen2) <br> [Tutorial: Extrair, transformar e carregar dados através do Azure Databricks](/azure/databricks/scenarios/databricks-extract-load-sql-data-warehouse) <br>[Tutorial: Acesso dados do Lago de Armazenamento De Dados Gen2 dados com Azure Databricks usando Spark](data-lake-storage-use-databricks-spark.md)|
|Hub de Eventos do Azure|Disponível em Geral|No|Yes|[Capture eventos através de Azure Event Hubs em Azure Blob Storage ou Azure Data Lake Storage](../../event-hubs/event-hubs-capture-overview.md)|
|Azure Event Grid|Disponível em Geral|Yes|Yes|[Tutorial: Implementar o padrão de captura do lago de dados para atualizar uma tabela Delta databricks](data-lake-storage-events.md)|
|Azure Logic Apps|Disponível em Geral|No|Yes|[Visão geral - O que é Azure Logic Apps?](../../logic-apps/logic-apps-overview.md)|
|Azure Machine Learning|Disponível em Geral|Yes|Yes|[Dados de acesso nos serviços de armazenamento Azure](../../machine-learning/how-to-access-data.md)|
|Azure Stream Analytics|Disponível em Geral|Yes|Yes|[Início Rápido: Criar uma tarefa do Stream Analytics com o portal do Azure](../../stream-analytics/stream-analytics-quick-create-portal.md) <br> [Egress para Azure Data Lake Gen2](../../stream-analytics/stream-analytics-define-outputs.md)|
|Data Box|Disponível em Geral|No|Yes|[Utilize a Caixa de Dados Azure para migrar dados de uma loja HDFS no local para o Azure Storage](data-lake-storage-migrate-on-premises-hdfs-cluster.md)|
|HDInsight |Disponível em Geral|Yes|Yes|[Utilizar o Azure Data Lake Storage Gen2 com clusters do Azure HDInsight](../../hdinsight/hdinsight-hadoop-use-data-lake-storage-gen2.md)<br>[Utilização do HDFS CLI com data lake storage gen2](data-lake-storage-use-hdfs-data-lake-storage.md) <br>[Tutorial: Extrair, transformar e carregar dados utilizando a Colmeia Apache em Azure HDInsight](data-lake-storage-tutorial-extract-transform-load-hive.md)|
|IoT Hub |Disponível em Geral|Yes|Yes|[Utilize o encaminhamento de mensagens IoT Hub para enviar mensagens dispositivo-a-nuvem para diferentes pontos finais](../../iot-hub/iot-hub-devguide-messages-d2c.md)|
|Power BI|Disponível em Geral|Yes|Yes|[Analise os dados em Data Lake Storage Gen2 usando Power BI](/power-query/connectors/datalakestorage)|
|Azure Synapse Analytics (anteriormente SQL Data Warehouse)|Disponível em Geral|Yes|Yes|[Analisar dados numa conta de armazenamento](../../synapse-analytics/get-started-analyze-storage.md)|
|SQL Server Integration Services (SSIS)|Disponível em Geral|Yes|Yes|[Gestor de conexão Azure Storage](/sql/integration-services/connection-manager/azure-storage-connection-manager)|
|Azure Data Explorer|Disponível em Geral|Yes|Yes|[Dados de consulta no Lago de Dados Azure usando O Explorador de Dados Azure](/azure/data-explorer/data-lake-query-data)|
|Azure Cognitive Search|Pré-visualizar|Yes|Yes|[Indexe e pesquisa documentos da Azure Data Lake Storage Gen2 (pré-visualização)](../../search/search-howto-index-azure-data-lake-storage.md)|
|Rede de Entrega de Conteúdos do Azure|Ainda não suportado|Não aplicável|Não aplicável|[Indexe e pesquisa documentos da Azure Data Lake Storage Gen2 (pré-visualização)](../../cdn/cdn-overview.md)|
|Base de Dados SQL do Azure|Ainda não suportado|Não aplicável|Não aplicável|[O que é a Base de Dados SQL do Azure?](../../azure-sql/database/sql-database-paas-overview.md)|

## <a name="see-also"></a>Ver também

- [Problemas conhecidos com Azure Data Lake Storage Gen2](data-lake-storage-known-issues.md)
- [Funcionalidades do armazenamento de blobs disponíveis no Azure Data Lake Storage Gen2](data-lake-storage-supported-blob-storage-features.md)
- [Plataformas open source que suportam Azure Data Lake Storage Gen2](data-lake-storage-supported-open-source-platforms.md)
- [Acesso multi-protocolo no Azure Data Lake Storage](data-lake-storage-multi-protocol-access.md)