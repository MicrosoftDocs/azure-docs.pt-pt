---
title: Ambientes computativos apoiados pela Azure Data Factory
description: Ambientes computativos que podem ser usados com oleodutos Azure Data Factory (como Azure HDInsight) para transformar ou processar dados.
ms.service: data-factory
ms.topic: conceptual
author: nabhishek
ms.author: abnarain
ms.date: 05/08/2019
ms.openlocfilehash: b9f7cce39e4f51aea7fc4db5ca37ee054f42c5cb
ms.sourcegitcommit: 3ee3045f6106175e59d1bd279130f4933456d5ff
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/31/2021
ms.locfileid: "106078660"
---
# <a name="compute-environments-supported-by-azure-data-factory"></a>Ambientes computativos apoiados pela Azure Data Factory

[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

Este artigo explica diferentes ambientes de computação que pode usar para processar ou transformar dados. Também fornece detalhes sobre diferentes configurações (a pedido vs. traga a sua própria) suportadas pela Data Factory ao configurar serviços ligados que liguem estes ambientes computativos a uma fábrica de dados Azure.

A tabela seguinte fornece uma lista de ambientes computativos apoiados pela Data Factory e as atividades que podem ser executadas neles. 

| Ambiente de computação                                          | Atividades                                                   |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| [Cluster HDInsight a pedido](#azure-hdinsight-on-demand-linked-service) ou [o seu próprio cluster HDInsight](#azure-hdinsight-linked-service) | [Colmeia,](transform-data-using-hadoop-hive.md) [Porco,](transform-data-using-hadoop-pig.md) [Faísca,](transform-data-using-spark.md) [MapReduce,](transform-data-using-hadoop-map-reduce.md) [Hadoop Streaming](transform-data-using-hadoop-streaming.md) |
| [Azure Batch](#azure-batch-linked-service)                   | [Personalizado](transform-data-using-dotnet-custom-activity.md)     |
| [Azure Machine Learning Studio (clássico)](#azure-machine-learning-studio-classic-linked-service) | [Atividades do Estúdio de Machine Learning (clássicos): Execução de Lote e Recurso de Atualização](transform-data-using-machine-learning.md) |
| [Azure Machine Learning](#azure-machine-learning-linked-service) | [Azure Machine Learning Executar Pipeline](transform-data-machine-learning-service.md) |
| [Azure Data Lake Analytics](#azure-data-lake-analytics-linked-service) | [Data Lake Analytics U-SQL](transform-data-using-data-lake-analytics.md) |
| [Azure SQL](#azure-sql-database-linked-service), [Azure Synapse Analytics,](#azure-synapse-analytics-linked-service) [SQL Server](#sql-server-linked-service) | [Procedimento Armazenado](transform-data-using-stored-procedure.md) |
| [Azure Databricks](#azure-databricks-linked-service)         | [Caderno,](transform-data-databricks-notebook.md) [Jar,](transform-data-databricks-jar.md) [Python](transform-data-databricks-python.md) |
| [Função do Azure](#azure-function-linked-service)         | [Atividade da Função Azure](control-flow-azure-function-activity.md)
>  

## <a name="hdinsight-compute-environment"></a>HdInsight ambiente de computação

Consulte a tabela abaixo para obter mais detalhes sobre os tipos de serviço ligados ao armazenamento suportado para configuração em ambiente on-demand e BYOC (Traga o seu próprio cálculo).

| No Serviço De Ligação computacional | Nome da Propriedade                | Description                                                  | Blobs | ADLS Gen2 | BD SQL do Azure | ADLS Gen 1 |
| ------------------------- | ---------------------------- | ------------------------------------------------------------ | ---- | --------- | ------------ | ---------- |
| A pedido                 | linkedServiceName            | Serviço ligado a Azure Storage a ser utilizado pelo cluster a pedido para armazenar e processar dados. | Yes  | Yes       | No           | No         |
|                           | adicionalLinkedServiceNames | Especifica contas de armazenamento adicionais para o serviço ligado ao HDInsight para que o serviço Data Factory possa registá-los em seu nome. | Yes  | No        | No           | No         |
|                           | hcatalogLinkedServiceName    | O nome do serviço ligado Azure SQL que aponta para a base de dados HCatalog. O cluster HDInsight a pedido é criado utilizando a base de dados Azure SQL como a metástaria. | No   | No        | Yes          | No         |
| BYOC                      | linkedServiceName            | A referência de serviço ligada ao Azure Storage.                | Yes  | Yes       | No           | No         |
|                           | adicionalLinkedServiceNames | Especifica contas de armazenamento adicionais para o serviço ligado ao HDInsight para que o serviço Data Factory possa registá-los em seu nome. | No   | No        | No           | No         |
|                           | hcatalogLinkedServiceName    | Uma referência ao serviço ligado Azure SQL que aponta para a base de dados HCatalog. | No   | No        | No           | No         |

### <a name="azure-hdinsight-on-demand-linked-service"></a>Serviço ligado do Azure HDInsight a pedido

Neste tipo de configuração, o ambiente de computação é totalmente gerido pelo serviço Azure Data Factory. É automaticamente criado pelo serviço Data Factory antes de um trabalho ser submetido para processar dados e removido quando o trabalho estiver concluído. Você pode criar um serviço ligado para o ambiente de computação on-demand, configurá-lo, e controlar configurações granulares para execução de emprego, gestão de clusters e ações de bootstrapping.

> [!NOTE]
> A configuração a pedido é atualmente suportada apenas para clusters Azure HDInsight. A Azure Databricks também apoia empregos a pedido utilizando clusters de emprego. Para obter mais informações, consulte [o serviço de dados da Azure databricks .](#azure-databricks-linked-service)

O serviço Azure Data Factory pode criar automaticamente um cluster HDInsight a pedido para processar dados. O cluster é criado na mesma região que a conta de armazenamento (propriedade linkedServiceName no JSON) associada ao cluster. A conta de armazenamento `must` é uma conta de armazenamento padrão de uso geral. 

Note os seguintes pontos **importantes** sobre o serviço ligado a hdinsight a pedido:

* O cluster HDInsight a pedido é criado sob a sua subscrição Azure. Você é capaz de ver o cluster no seu portal Azure quando o cluster está em funcionamento. 
* Os registos de empregos que são executados num cluster HDInsight a pedido são copiados para a conta de armazenamento associada ao cluster HDInsight. O clusterUserName, clusterPassword, clusterSshUserName, clusterSshPassword definido na definição de serviço ligado são usados para iniciar sessão no cluster para uma resolução aprofundada de problemas durante o ciclo de vida do cluster. 
* É cobrado apenas pelo momento em que o cluster HDInsight está a funcionar.
* Você pode usar uma **Ação de Script** com o serviço Azure HDInsight on demand.  

> [!IMPORTANT]
> Normalmente leva **20 minutos** ou mais para providenciar um cluster Azure HDInsight a pedido.

#### <a name="example"></a>Exemplo

O JSON seguinte define um serviço hdinsight baseado na procura do Linux. O serviço Data Factory cria automaticamente um cluster HDInsight **baseado em Linux** para processar a atividade necessária. 

```json
{
  "name": "HDInsightOnDemandLinkedService",
  "properties": {
    "type": "HDInsightOnDemand",
    "typeProperties": {
      "clusterType": "hadoop",
      "clusterSize": 1,
      "timeToLive": "00:15:00",
      "hostSubscriptionId": "<subscription ID>",
      "servicePrincipalId": "<service principal ID>",
      "servicePrincipalKey": {
        "value": "<service principal key>",
        "type": "SecureString"
      },
      "tenant": "<tenent id>",
      "clusterResourceGroup": "<resource group name>",
      "version": "3.6",
      "osType": "Linux",
      "linkedServiceName": {
        "referenceName": "AzureStorageLinkedService",
        "type": "LinkedServiceReference"
      }
    },
    "connectVia": {
      "referenceName": "<name of Integration Runtime>",
      "type": "IntegrationRuntimeReference"
    }
  }
}
```

> [!IMPORTANT]
> O cluster HDInsight cria um **recipiente predefinido** no armazenamento de bolhas especificado no JSON **(linkedServiceName**). Quando o cluster é eliminado, o HDInsight não é eliminado deste contentor. Este comportamento é propositado. Com o serviço ligado do HDInsight a pedido, é criado um cluster do HDInsight sempre que um setor tiver de ser processado, exceto se houver um cluster em direto (**timeToLive**) que será eliminado no fim do processamento. 
>
> À medida que mais atividade corre, você vê muitos recipientes no seu armazenamento de bolhas Azure. Se não precisar deles para a resolução de problemas das tarefas, poderá eliminá-los para reduzir o custo de armazenamento. Os nomes destes contentores seguem um padrão: `adf**yourdatafactoryname**-**linkedservicename**-datetimestamp`. Utilize ferramentas como o [Microsoft Azure Storage Explorer](https://storageexplorer.com/) para eliminar recipientes no seu armazenamento de bolhas Azure.

#### <a name="properties"></a>Propriedades

| Propriedade                     | Descrição                              | Obrigatório |
| ---------------------------- | ---------------------------------------- | -------- |
| tipo                         | A propriedade tipo deve ser definida para **HDInsightOnDemand**. | Yes      |
| clusterSize                  | Número de nós de trabalhador/dados no cluster. O cluster HDInsight é criado com 2 nós de cabeça juntamente com o número de nós de trabalhadores que especifica para esta propriedade. Os nódinhos são de tamanho Standard_D3 que tem 4 núcleos, por isso um conjunto de nó de 4 trabalhadores leva 24 \* núcleos (4 4 = 16 núcleos para os nóns operários, mais \* 2 4 = 8 núcleos para os nosdes da cabeça). Consulte [configurar clusters em HDInsight com Hadoop, Spark, Kafka e mais](../hdinsight/hdinsight-hadoop-provision-linux-clusters.md) detalhes. | Yes      |
| linkedServiceName            | Serviço ligado a Azure Storage a ser utilizado pelo cluster a pedido para armazenar e processar dados. O cluster HDInsight é criado na mesma região que esta conta de Armazenamento Azure. O Azure HDInsight tem limitação do número total de núcleos que pode utilizar em cada região do Azure que suporta. Certifique-se de que tem quotas nucleares suficientes nessa região de Azure para atender ao clusters necessários. Para mais detalhes, consulte [configurar clusters em HDInsight com Hadoop, Spark, Kafka, e muito mais](../hdinsight/hdinsight-hadoop-provision-linux-clusters.md)<p>Atualmente, não é possível criar um cluster HDInsight a pedido que utilize um Azure Data Lake Storage (Gen 2) como armazenamento. Se pretender armazenar os dados dos resultados do processamento de HDInsight num Armazenamento do Lago de Dados Azure (Gen 2), utilize uma Atividade de Cópia para copiar os dados do Armazenamento da Bolha Azure para o Armazenamento do Lago de Dados Azure (Gen 2). </p> | Yes      |
| clusterResourceGroup         | O cluster HDInsight é criado neste grupo de recursos. | Yes      |
| timetolive                   | O tempo de inatividade permitido para o cluster HDInsight a pedido. Especifica quanto tempo o cluster HDInsight on-demand permanece vivo após a conclusão de uma atividade executada se não houver outros empregos ativos no cluster. O valor mínimo permitido é de 5 minutos (00:05:00).<br/><br/>Por exemplo, se uma corrida de atividade demorar 6 minutos e o tempo de vida estiver definido para 5 minutos, o cluster permanece vivo durante 5 minutos após os 6 minutos de processamento da atividade. Se outra atividade for executada com a janela de 6 minutos, é processada pelo mesmo cluster.<br/><br/>A criação de um cluster HDInsight a pedido é uma operação dispendiosa (pode demorar algum tempo), por isso use esta definição como necessário para melhorar o desempenho de uma fábrica de dados reutilizando um cluster HDInsight a pedido.<br/><br/>Se definir o valor de live para 0, o cluster é eliminado assim que a atividade termina. Enquanto que, se definir um valor elevado, o cluster pode ficar inativo para que você faça login para algum propósito de resolução de problemas, mas pode resultar em custos elevados. Por isso, é importante que desaproprie o valor adequado com base nas suas necessidades.<br/><br/>Se o valor da propriedade timetolive for devidamente definido, vários oleodutos podem partilhar a instância do cluster HDInsight a pedido. | Yes      |
| clusterType                  | O tipo de cluster HDInsight a ser criado. Os valores permitidos são "hadoop" e "faísca". Se não for especificado, o valor predefinido é hadoop. O cluster habilitado para o Pacote de Segurança Empresarial não pode ser criado a pedido, em vez de utilizar um [cluster existente/ traga o seu próprio cálculo](#azure-hdinsight-linked-service). | No       |
| versão                      | Versão do cluster HDInsight. Se não for especificado, está a utilizar a versão padrão definida por HDInsight atual. | No       |
| hostSubscriptionId           | O ID de subscrição Azure usado para criar cluster HDInsight. Se não for especificado, utiliza o ID de subscrição do seu contexto de login Azure. | No       |
| clusterNamePrefixo           | O prefixo do nome do cluster HDI, um tempotamp automaticamente anexa no final do nome do cluster| No       |
| sparkVersion                 | A versão de faísca se o tipo de cluster é "Faísca" | No       |
| adicionalLinkedServiceNames | Especifica contas de armazenamento adicionais para o serviço ligado ao HDInsight para que o serviço Data Factory possa registá-los em seu nome. Estas contas de armazenamento devem situar-se na mesma região que o cluster HDInsight, que é criado na mesma região que a conta de armazenamento especificada pelo linkedServiceName. | No       |
| osTipos                       | Tipo de sistema operativo. Os valores permitidos são: Linux e Windows (apenas para HDInsight 3.3). O padrão é Linux. | No       |
| hcatalogLinkedServiceName    | O nome do serviço ligado Azure SQL que aponta para a base de dados HCatalog. O cluster HDInsight a pedido é criado utilizando a Base de Dados Azure SQL como a metástaria. | No       |
| connectVia                   | O Tempo de Execução de Integração a ser utilizado para despachar as atividades para este serviço ligado ao HDInsight. Para um serviço on-demand HDInsight ligado, ele apenas suporta O Tempo de Execução de Integração Azure. Se não for especificado, utiliza o tempo de execução de integração Azure predefinido. | No       |
| clusterUserName                   | O nome de utilizador para aceder ao cluster. | No       |
| clusterPassword                   | A palavra-passe no tipo de cadeia segura para aceder ao cluster. | No       |
| clusterSshUserName         | O nome de utilizador do SSH liga-se remotamente ao nó do cluster (para o Linux). | No       |
| clusterSshPassword         | A palavra-passe no tipo de cadeia segura ao SSH liga remotamente o nó do cluster (para o Linux). | No       |
| scriptActions | Especifique o script para [personalizações de cluster HDInsight](../hdinsight/hdinsight-hadoop-customize-cluster-linux.md) durante a criação de clusters a pedido. <br />Atualmente, a ferramenta de autoria da Interface de Utilizador da Azure Data Factory suporta especificar apenas 1 ação de script, mas pode passar por esta limitação no JSON (especificar várias ações de script no JSON). | No |


> [!IMPORTANT]
> O HDInsight suporta várias versões de cluster Hadoop que podem ser implementadas. Cada escolha de versão cria uma versão específica da distribuição da Plataforma de Dados Hortonworks (HDP) e um conjunto de componentes que estão contidos nessa distribuição. A lista de versões HDInsight suportadas continua a ser atualizada para fornecer os mais recentes componentes e correções do ecossistema Hadoop. Certifique-se de que se refere sempre às informações mais recentes da [versão HDInsight suportada e do tipo OS](../hdinsight/hdinsight-component-versioning.md#supported-hdinsight-versions) para garantir que está a utilizar a versão suportada do HDInsight. 
>
> [!IMPORTANT]
> Atualmente, os serviços ligados ao HDInsight não suportam HBase, Interactive Query (Hive LLAP), Storm. 

* adicionalLinkedServiceNames JSON exemplo

```json
"additionalLinkedServiceNames": [{
    "referenceName": "MyStorageLinkedService2",
    "type": "LinkedServiceReference"          
}]
```

#### <a name="service-principal-authentication"></a>Autenticação do principal de serviço

O serviço on-demand HDInsight ligado requer uma autenticação principal do serviço para criar clusters HDInsight em seu nome. Para utilizar a autenticação principal do serviço, registe uma entidade de aplicação no Azure Ative Directory (Azure AD) e conceda-lhe o papel **de Contribuinte** da subscrição ou do grupo de recursos em que o cluster HDInsight é criado. Para etapas detalhadas, consulte [o portal Use para criar uma aplicação e um diretor de serviço azure ative que possa aceder aos recursos.](../active-directory/develop/howto-create-service-principal-portal.md) Tome nota dos seguintes valores, que utiliza para definir o serviço ligado:

- ID da Aplicação
- Chave de aplicação 
- ID do inquilino

Utilize a autenticação principal do serviço especificando as seguintes propriedades:

| Propriedade                | Descrição                              | Obrigatório |
| :---------------------- | :--------------------------------------- | :------- |
| **servicePrincipalId**  | Especifique a identificação do cliente da aplicação.     | Yes      |
| **servicePrincipalKey** | Especifique a chave da aplicação.           | Yes      |
| **tenant**              | Especifique a informação do inquilino (nome de domínio ou ID do inquilino) sob a qual a sua aplicação reside. Pode recuperá-lo pairando sobre o rato no canto superior direito do portal Azure. | Yes      |

#### <a name="advanced-properties"></a>Propriedades Avançadas

Também pode especificar as seguintes propriedades para a configuração granular do cluster HDInsight a pedido.

| Propriedade               | Descrição                              | Obrigatório |
| :--------------------- | :--------------------------------------- | :------- |
| coreConfiguration      | Especifica os parâmetros de configuração do núcleo (como em core-site.xml) para a criação do cluster HDInsight. | No       |
| hConfiguration base     | Especifica os parâmetros de configuração HBase (hbase-site.xml) para o cluster HDInsight. | No       |
| hdfsConfiguration      | Especifica os parâmetros de configuração HDFS (hdfs-site.xml) para o cluster HDInsight. | No       |
| hiveConfiguration      | Especifica os parâmetros de configuração da colmeia (hive-site.xml) para o cluster HDInsight. | No       |
| mapReduceConfiguration | Especifica os parâmetros de configuração mapReduce (mapred-site.xml) para o cluster HDInsight. | No       |
| oozieConfiguration     | Especifica os parâmetros de configuração Oozie (oozie-site.xml) para o cluster HDInsight. | No       |
| tempestadeConfiguration     | Especifica os parâmetros de configuração da tempestade (storm-site.xml) para o cluster HDInsight. | No       |
| yarConfiguration      | Especifica os parâmetros de configuração do fio (yarn-site.xml) para o cluster HDInsight. | No       |

* Exemplo – Configuração de cluster HDInsight a pedido com propriedades avançadas

```json
{
    "name": " HDInsightOnDemandLinkedService",
    "properties": {
      "type": "HDInsightOnDemand",
      "typeProperties": {
          "clusterSize": 16,
          "timeToLive": "01:30:00",
          "hostSubscriptionId": "<subscription ID>",
          "servicePrincipalId": "<service principal ID>",
          "servicePrincipalKey": {
            "value": "<service principal key>",
            "type": "SecureString"
          },
          "tenant": "<tenent id>",
          "clusterResourceGroup": "<resource group name>",
          "version": "3.6",
          "osType": "Linux",
          "linkedServiceName": {
              "referenceName": "AzureStorageLinkedService",
              "type": "LinkedServiceReference"
            },
            "coreConfiguration": {
                "templeton.mapper.memory.mb": "5000"
            },
            "hiveConfiguration": {
                "templeton.mapper.memory.mb": "5000"
            },
            "mapReduceConfiguration": {
                "mapreduce.reduce.java.opts": "-Xmx4000m",
                "mapreduce.map.java.opts": "-Xmx4000m",
                "mapreduce.map.memory.mb": "5000",
                "mapreduce.reduce.memory.mb": "5000",
                "mapreduce.job.reduce.slowstart.completedmaps": "0.8"
            },
            "yarnConfiguration": {
                "yarn.app.mapreduce.am.resource.mb": "5000",
                "mapreduce.map.memory.mb": "5000"
            },
            "additionalLinkedServiceNames": [{
                "referenceName": "MyStorageLinkedService2",
                "type": "LinkedServiceReference"          
            }]
        }
    },
      "connectVia": {
      "referenceName": "<name of Integration Runtime>",
      "type": "IntegrationRuntimeReference"
    }
}
```

#### <a name="node-sizes"></a>Tamanhos dos nómdais
Pode especificar os tamanhos dos nosmos de cabeça, dados e zookeeper utilizando as seguintes propriedades: 

| Propriedade          | Descrição                              | Obrigatório |
| :---------------- | :--------------------------------------- | :------- |
| headNodeSize      | Especifica o tamanho do nó da cabeça. O valor predefinido é: Standard_D3. Consulte a secção **de tamanhos dos nós especificando** para obter mais detalhes. | No       |
| dataNodeSize      | Especifica o tamanho do nó de dados. O valor predefinido é: Standard_D3. | No       |
| zookeeperNodeSize | Especifica o tamanho do nó do Guardião do Jardim Zoológico. O valor predefinido é: Standard_D3. | No       |

* Especificar tamanhos de nós Ver os [tamanhos das máquinas virtuais](../virtual-machines/sizes.md) artigo para valores de cadeia que precisa de especificar para as propriedades mencionadas na secção anterior. Os valores devem estar em conformidade com os **CMDLETs & APIS** referenciados no artigo. Como pode ver no artigo, o nó de dados do tamanho grande (padrão) tem memória de 7 GB, o que pode não ser bom o suficiente para o seu cenário. 

Se pretender criar nós de cabeça de tamanho D4 e nós de trabalhadores, especifique **Standard_D4** como o valor para propriedades headNodeSize e dataNodeSize. 

```json
"headNodeSize": "Standard_D4",    
"dataNodeSize": "Standard_D4",
```

Se especificar um valor errado para estas propriedades, poderá receber o seguinte **erro:** Não conseguiu criar o cluster. Exceção: não foi possível concluir a operação de criação do cluster. A operação falhou com o código "400". Estado do cluster não concluído: "Erro". Mensagem: 'PreClusterCreationValidationFailure'. Quando receber este erro, certifique-se de que está a utilizar o nome **CMDLET & APIS** da tabela no artigo [Tamanhos das Máquinas Virtuais.](../virtual-machines/sizes.md)

### <a name="bring-your-own-compute-environment"></a>Traga o seu próprio ambiente computacional
Neste tipo de configuração, os utilizadores podem registar um ambiente de computação já existente como um serviço ligado na Data Factory. O ambiente de computação é gerido pelo utilizador e o serviço Data Factory utiliza-o para executar as atividades.

Este tipo de configuração é suportado para os seguintes ambientes computativos:

* Azure HDInsight
* Azure Batch
* Azure Machine Learning
* Azure Data Lake Analytics
* Azure SQL DB, Azure Synapse Analytics, SQL Server

## <a name="azure-hdinsight-linked-service"></a>Serviço ligado a Azure HDInsight
Pode criar um serviço ligado a Azure HDInsight para registar o seu próprio cluster HDInsight com data factory.

### <a name="example"></a>Exemplo

```json
{
    "name": "HDInsightLinkedService",
    "properties": {
      "type": "HDInsight",
      "typeProperties": {
        "clusterUri": " https://<hdinsightclustername>.azurehdinsight.net/",
        "userName": "username",
        "password": {
            "value": "passwordvalue",
            "type": "SecureString"
          },
        "linkedServiceName": {
              "referenceName": "AzureStorageLinkedService",
              "type": "LinkedServiceReference"
        }
      },
      "connectVia": {
        "referenceName": "<name of Integration Runtime>",
        "type": "IntegrationRuntimeReference"
      }
    }
  }
```

### <a name="properties"></a>Propriedades
| Propriedade          | Descrição                                                  | Obrigatório |
| ----------------- | ------------------------------------------------------------ | -------- |
| tipo              | A propriedade tipo deve ser definida para **HDInsight**.            | Yes      |
| clusterUri        | O URI do cluster HDInsight.                            | Yes      |
| nome de utilizador          | Especifique o nome do utilizador a utilizar para ligar a um cluster HDInsight existente. | Yes      |
| palavra-passe          | Especifique a palavra-passe para a conta de utilizador.                       | Yes      |
| linkedServiceName | Nome do serviço ligado ao Azure Storage que se refere ao armazenamento de bolhas Azure utilizado pelo cluster HDInsight. <p>Atualmente, não é possível especificar um serviço de armazenamento de dados Azure (Gen 2) ligado a esta propriedade. Se o cluster HDInsight tiver acesso à Data Lake Store, poderá aceder a dados no Azure Data Lake Storage (Gen 2) a partir de scripts Hive/Pig. </p> | Yes      |
| isEspEnabled      | Especifique '*verdadeiro*' se o cluster HDInsight for [o Pacote de Segurança Empresarial](../hdinsight/domain-joined/apache-domain-joined-architecture.md) ativado. O padrão é *falso.* | No       |
| connectVia        | O Tempo de Integração a ser utilizado para despachar as atividades para este serviço ligado. Pode utilizar o Tempo de Execução da Integração Azure ou o Tempo de Execução de Integração Auto-hospedado. Se não for especificado, utiliza o tempo de execução de integração Azure predefinido. <br />Para o Pacote de Segurança Empresarial (ESP) habilitado o cluster HDInsight utilizar um tempo de integração auto-hospedado, que tem uma linha de visão para o cluster ou deve ser implantado dentro da mesma Rede Virtual que o cluster ESP HDInsight. | No       |

> [!IMPORTANT]
> O HDInsight suporta várias versões de cluster Hadoop que podem ser implementadas. Cada escolha de versão cria uma versão específica da distribuição da Plataforma de Dados Hortonworks (HDP) e um conjunto de componentes que estão contidos nessa distribuição. A lista de versões HDInsight suportadas continua a ser atualizada para fornecer os mais recentes componentes e correções do ecossistema Hadoop. Certifique-se de que se refere sempre às informações mais recentes da [versão HDInsight suportada e do tipo OS](../hdinsight/hdinsight-component-versioning.md#supported-hdinsight-versions) para garantir que está a utilizar a versão suportada do HDInsight. 
>
> [!IMPORTANT]
> Atualmente, os serviços ligados ao HDInsight não suportam HBase, Interactive Query (Hive LLAP), Storm. 
>
> 

## <a name="azure-batch-linked-service"></a>Serviço ligado a Azure Batch

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Pode criar um serviço ligado a Azure Batch para registar um conjunto de máquinas virtuais (VMs) numa fábrica de dados. Pode executar a atividade personalizada utilizando o Azure Batch.

Consulte os seguintes artigos se é novo no serviço Azure Batch:

* [Básicos do Azure Batch](../batch/batch-technical-overview.md) para uma visão geral do serviço Azure Batch.
* [New-AzBatchAccount](/powershell/module/az.batch/New-azBatchAccount) cmdlet para criar uma conta Azure Batch (ou) [portal Azure](../batch/batch-account-create-portal.md) para criar a conta Azure Batch utilizando o portal Azure. Consulte [a Utilização do PowerShell para gerir o artigo da Conta de Lote Azure](/archive/blogs/windowshpc/using-azure-powershell-to-manage-azure-batch-account) para obter instruções detalhadas sobre a utilização do cmdlet.
* [Cmdlet New-AzBatchPool](/powershell/module/az.batch/New-AzBatchPool) para criar uma piscina Azure Batch.

> [!IMPORTANT]
> Ao criar um novo pool Azure Batch, deve ser utilizado o 'VirtualMachineConfiguration' e NÃO 'CloudServiceConfiguration'. Para mais detalhes, consulte [a orientação de migração do Azure Batch Pool.](../batch/batch-pool-cloud-service-to-virtual-machine-configuration.md) 

### <a name="example"></a>Exemplo

```json
{
    "name": "AzureBatchLinkedService",
    "properties": {
      "type": "AzureBatch",
      "typeProperties": {
        "accountName": "batchaccount",
        "accessKey": {
          "type": "SecureString",
          "value": "access key"
        },
        "batchUri": "https://batchaccount.region.batch.azure.com",
        "poolName": "poolname",
        "linkedServiceName": {
          "referenceName": "StorageLinkedService",
          "type": "LinkedServiceReference"
        }
      },
      "connectVia": {
        "referenceName": "<name of Integration Runtime>",
        "type": "IntegrationRuntimeReference"
      }
    }
  }
```


### <a name="properties"></a>Propriedades
| Propriedade          | Descrição                              | Obrigatório |
| ----------------- | ---------------------------------------- | -------- |
| tipo              | A propriedade tipo deve ser definida para **AzureBatch**. | Yes      |
| accountName       | Nome da conta Azure Batch.         | Yes      |
| acessoKey         | Chave de acesso para a conta Azure Batch.  | Yes      |
| batchUri          | URL para a sua conta Azure Batch, em formato de https://*batchaccountname.região*.batch.azure.com. | Yes      |
| poolName          | Nome da piscina de máquinas virtuais.    | Yes      |
| linkedServiceName | Nome do serviço ligado ao Azure Storage associado a este serviço ligado a Azure Batch. Este serviço ligado é utilizado para a realização de ficheiros necessários para executar a atividade. | Yes      |
| connectVia        | O Tempo de Integração a ser utilizado para despachar as atividades para este serviço ligado. Pode utilizar o Tempo de Execução da Integração Azure ou o Tempo de Execução de Integração Auto-hospedado. Se não for especificado, utiliza o tempo de execução de integração Azure predefinido. | No       |

## <a name="azure-machine-learning-studio-classic-linked-service"></a>Azure Machine Learning Studio (clássico) serviço ligado
Você cria um serviço ligado ao Azure Machine Learning Studio (clássico) para registar um ponto final de pontuação de um estúdio de machine learning (clássico) para uma fábrica de dados.

### <a name="example"></a>Exemplo

```json
{
    "name": "AzureMLLinkedService",
    "properties": {
      "type": "AzureML",
      "typeProperties": {
        "mlEndpoint": "https://[batch scoring endpoint]/jobs",
        "apiKey": {
            "type": "SecureString",
            "value": "access key"
        }
     },
     "connectVia": {
        "referenceName": "<name of Integration Runtime>",
        "type": "IntegrationRuntimeReference"
      }
    }
}
```

### <a name="properties"></a>Propriedades
| Propriedade               | Descrição                              | Necessário                                 |
| ---------------------- | ---------------------------------------- | ---------------------------------------- |
| Tipo                   | A propriedade tipo deve ser definida para: **AzureML**. | Yes                                      |
| mlEndpoint             | O URL de pontuação do lote.                   | Yes                                      |
| apiKey                 | A API do modelo de espaço de trabalho publicado.     | Yes                                      |
| updateResourceEndpoint | O URL de recurso de atualização para um estúdio de aprendizagem de máquinas Azure (clássico) do Serviço Web usado para atualizar o serviço web preditivo com um ficheiro de modelo treinado | No                                       |
| servicePrincipalId     | Especifique a identificação do cliente da aplicação.     | Necessário se a actualizaçãoResourceEndpoint for especificado |
| servicePrincipalKey    | Especifique a chave da aplicação.           | Necessário se a actualizaçãoResourceEndpoint for especificado |
| inquilino                 | Especifique a informação do inquilino (nome de domínio ou ID do inquilino) sob a qual a sua aplicação reside. Pode recuperá-lo pairando sobre o rato no canto superior direito do portal Azure. | Necessário se a actualizaçãoResourceEndpoint for especificado |
| connectVia             | O Tempo de Integração a ser utilizado para despachar as atividades para este serviço ligado. Pode utilizar o Tempo de Execução da Integração Azure ou o Tempo de Execução de Integração Auto-hospedado. Se não for especificado, utiliza o tempo de execução de integração Azure predefinido. | No                                       |

## <a name="azure-machine-learning-linked-service"></a>Serviço ligado a Azure Machine Learning
Você cria um serviço ligado a Azure Machine Learning para ligar um espaço de trabalho Azure Machine Learning a uma fábrica de dados.

> [!NOTE]
> Atualmente apenas a autenticação principal do serviço é suportada para o serviço Azure Machine Learning ligado.

### <a name="example"></a>Exemplo

```json
{
    "name": "AzureMLServiceLinkedService",
    "properties": {
        "type": "AzureMLService",
        "typeProperties": {
            "subscriptionId": "subscriptionId",
            "resourceGroupName": "resourceGroupName",
            "mlWorkspaceName": "mlWorkspaceName",
            "servicePrincipalId": "service principal id",
            "servicePrincipalKey": {
                "value": "service principal key",
                "type": "SecureString"
            },
            "tenant": "tenant ID"
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime?",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

### <a name="properties"></a>Propriedades

| Propriedade               | Descrição                              | Necessário                                 |
| ---------------------- | ---------------------------------------- | ---------------------------------------- |
| Tipo                   | A propriedade tipo deve ser configurada para: **AzureMLService**. | Yes                                      |
| subscriptionId         | ID de assinatura Azure              | Yes                                      |
| resourceGroupName      | name | Yes                                      |
| mlWorkspaceName        | Nome do espaço de trabalho Azure Machine Learning | Yes  |
| servicePrincipalId     | Especifique a identificação do cliente da aplicação.     | Yes |
| servicePrincipalKey    | Especifique a chave da aplicação.           | Yes |
| inquilino                 | Especifique a informação do inquilino (nome de domínio ou ID do inquilino) sob a qual a sua aplicação reside. Pode recuperá-lo pairando sobre o rato no canto superior direito do portal Azure. | Necessário se a actualizaçãoResourceEndpoint for especificado |
| connectVia             | O Tempo de Integração a ser utilizado para despachar as atividades para este serviço ligado. Pode utilizar o Tempo de Execução da Integração Azure ou o Tempo de Execução de Integração Auto-hospedado. Se não for especificado, utiliza o tempo de execução de integração Azure predefinido. | No |

## <a name="azure-data-lake-analytics-linked-service"></a>Serviço ligado a Azure Data Lake Analytics
Você cria um serviço **Azure Data Lake Analytics** ligado para ligar um serviço de computação Azure Data Lake Analytics a uma fábrica de dados Azure. A atividade U-SQL do Data Lake Analytics no oleoduto refere-se a este serviço ligado. 

### <a name="example"></a>Exemplo

```json
{
    "name": "AzureDataLakeAnalyticsLinkedService",
    "properties": {
        "type": "AzureDataLakeAnalytics",
        "typeProperties": {
            "accountName": "adftestaccount",
            "dataLakeAnalyticsUri": "azuredatalakeanalytics URI",
            "servicePrincipalId": "service principal id",
            "servicePrincipalKey": {
                "value": "service principal key",
                "type": "SecureString"
            },
            "tenant": "tenant ID",
            "subscriptionId": "<optional, subscription ID of ADLA>",
            "resourceGroupName": "<optional, resource group name of ADLA>"
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

### <a name="properties"></a>Propriedades

| Propriedade             | Descrição                              | Obrigatório                                 |
| -------------------- | ---------------------------------------- | ---------------------------------------- |
| tipo                 | A propriedade tipo deve ser configurada para: **AzureDataLakeAnalytics**. | Yes                                      |
| accountName          | Nome da conta Azure Data Lake Analytics.  | Yes                                      |
| dataLakeAnalyticsUri | Azure Data Lake Analytics URI.           | No                                       |
| subscriptionId       | ID de assinatura Azure                    | No                                       |
| resourceGroupName    | Nome do grupo de recursos do Azure                | No                                       |
| servicePrincipalId   | Especifique a identificação do cliente da aplicação.     | Yes                                      |
| servicePrincipalKey  | Especifique a chave da aplicação.           | Yes                                      |
| inquilino               | Especifique a informação do inquilino (nome de domínio ou ID do inquilino) sob a qual a sua aplicação reside. Pode recuperá-lo pairando sobre o rato no canto superior direito do portal Azure. | Yes                                      |
| connectVia           | O Tempo de Integração a ser utilizado para despachar as atividades para este serviço ligado. Pode utilizar o Tempo de Execução da Integração Azure ou o Tempo de Execução de Integração Auto-hospedado. Se não for especificado, utiliza o tempo de execução de integração Azure predefinido. | No                                       |



## <a name="azure-databricks-linked-service"></a>Serviço ligado Azure Databricks
Pode criar **o serviço ligado a Azure Databricks** para registar o espaço de trabalho databricks que utiliza para executar as cargas de trabalho databricks (caderno, frasco, pitão). 
> [!IMPORTANT]
> Os serviços ligados à Databricks suportam [agrupamentos de instância &](https://aka.ms/instance-pools) autenticação de identidade gerida atribuída pelo Sistema.

### <a name="example---using-new-job-cluster-in-databricks"></a>Exemplo - Utilização de novo cluster de emprego em Databricks

```json
{
    "name": "AzureDatabricks_LS",
    "properties": {
        "type": "AzureDatabricks",
        "typeProperties": {
            "domain": "https://eastus.azuredatabricks.net",
            "newClusterNodeType": "Standard_D3_v2",
            "newClusterNumOfWorker": "1:10",
            "newClusterVersion": "4.0.x-scala2.11",
            "accessToken": {
                "type": "SecureString",
                "value": "dapif33c9c721144c3a790b35000b57f7124f"
            }
        }
    }
}

```

### <a name="example---using-existing-interactive-cluster-in-databricks"></a>Exemplo - Utilização do cluster interativo existente em Databricks

```json
{
    "name": " AzureDataBricksLinedService",
    "properties": {
      "type": " AzureDatabricks",
      "typeProperties": {
        "domain": "https://westeurope.azuredatabricks.net",
        "accessToken": {
            "type": "SecureString", 
            "value": "dapif33c9c72344c3a790b35000b57f7124f"
          },
        "existingClusterId": "{clusterId}"
        }
}

```

### <a name="properties"></a>Propriedades

| Propriedade             | Descrição                              | Necessário                                 |
| -------------------- | ---------------------------------------- | ---------------------------------------- |
| name                 | Nome do Serviço Ligado               | Yes   |
| tipo                 | A propriedade tipo deve ser definida para: **Azure Databricks**. | Yes                                      |
| domínio               | Especificar a Região Azure em conformidade com base na região do espaço de trabalho Databricks. Exemplo: https://eastus.azuredatabricks.net | Yes                                 |
| accessToken          | O token de acesso é necessário para que a Data Factory autente para a Azure Databricks. O token de acesso precisa de ser gerado a partir do espaço de trabalho dos dados. Passos mais detalhados para encontrar o token de acesso podem ser encontrados [aqui](/azure/databricks/dev-tools/api/latest/authentication#generate-token)  | No                                       |
| MSI          | Utilize a identidade gerida da Data Factory (atribuída ao sistema) para autenticar a Azure Databricks. Não precisa do Access Token ao utilizar a autenticação 'MSI'  | No                                       |
| ClusterId existente    | ID do cluster de um cluster existente para executar todos os empregos nisto. Este deve ser um Cluster Interativo já criado. Pode ser necessário reiniciar manualmente o cluster se deixar de responder. Databricks sugerem trabalhar em novos clusters para uma maior fiabilidade. Pode encontrar o ID do Cluster de um Cluster Interativo no espaço de trabalho databricks -> Clusters - > Configuração de > Configuração ->. [Mais detalhes](https://docs.databricks.com/user-guide/clusters/tags.html) | No 
| instânciaPoolId    | Exemplo Pool ID de um pool existente no espaço de trabalho databricks.  | No  |
| newClusterVersion    | A versão Spark do cluster. Cria um aglomerado de emprego em databricks. | No  |
| newClusterNumOfWorker| Número de nós de trabalhadores que este aglomerado deve ter. Um cluster tem um Spark Driver e executores num_workers para um total de num_workers + 1 nós de faísca. Uma cadeia formatada Int32, como "1" significa que numOfWorker é 1 ou "1:10" significa autoescala a partir de 1 como min e 10 como máx.  | No                |
| newClusterNodeType   | Este campo codifica, através de um único valor, os recursos disponíveis para cada um dos nós Spark neste cluster. Por exemplo, os nós Spark podem ser a provisionados e otimizados para a memória ou calcular cargas de trabalho intensivas. Este campo é necessário para novo cluster                | No               |
| newClusterSparkConf  | um conjunto de pares opcionais de configuração de chaves de configuração de faíscas especificados pelo utilizador. Os utilizadores também podem passar uma série de opções JVM extra para o condutor e os executores através de spark.driver.extraJavaOptions e spark.executor.extraJavaOptions, respectivamente. | No  |
| newClusterInitScripts| um conjunto de scripts de inicialização opcional e definidos pelo utilizador para o novo cluster. Especificando o caminho DBFS para os scripts init. | No  |


## <a name="azure-sql-database-linked-service"></a>Serviço ligado da Base de Dados SQL do Azure

Cria um serviço ligado Azure SQL e utiliza-o com a [Atividade de Procedimento Armazenado](transform-data-using-stored-procedure.md) para invocar um procedimento armazenado a partir de um oleoduto data factory. Consulte o artigo [do Conector Azure SQL](connector-azure-sql-database.md#linked-service-properties) para obter mais informações sobre este serviço ligado.

## <a name="azure-synapse-analytics-linked-service"></a>Serviço Azure Synapse Analytics ligado

Cria um serviço Azure Synapse Analytics ligado e utiliza-o com a [Atividade de Procedimento Armazenado](transform-data-using-stored-procedure.md) para invocar um procedimento armazenado a partir de um oleoduto data factory. Consulte o artigo [do Azure Synapse Analytics Connector](connector-azure-sql-data-warehouse.md#linked-service-properties) para obter mais detalhes sobre este serviço ligado.

## <a name="sql-server-linked-service"></a>Serviço ligado ao SQL Server

Cria um serviço ligado ao SQL Server e utiliza-o com a [Atividade de Procedimento Armazenado](transform-data-using-stored-procedure.md) para invocar um procedimento armazenado a partir de um pipeline da Data Factory. Consulte o artigo [do conector SQL Server](connector-sql-server.md#linked-service-properties) para obter mais informações sobre este serviço ligado.

## <a name="azure-function-linked-service"></a>Serviço ligado à função Azure

Cria um serviço ligado à Função Azure e utiliza-o com a [atividade da Função Azure](control-flow-azure-function-activity.md) para executar funções Azure num pipeline da Data Factory. O tipo de devolução da função Azure tem de ser válido `JObject` . (Tenha em mente que [JArray](https://www.newtonsoft.com/json/help/html/T_Newtonsoft_Json_Linq_JArray.htm) *não* é `JObject` um.) Qualquer tipo de devolução que não `JObject` falhe e eleva o conteúdo de resposta ao erro do utilizador *não é um JObject válido*.

| **Propriedade** | **Descrição** | **Obrigatório** |
| --- | --- | --- |
| tipo   | A propriedade tipo deve ser definida para: **AzureFunction** | sim |
| url de aplicação de função | URL para a App de Função Azure. O formato é `https://<accountname>.azurewebsites.net` . Este URL é o valor na secção **URL** ao visualizar a sua App de Função no portal Azure  | sim |
| chave de função | Chave de acesso para a Função Azure. Clique na secção **'Gerir'** para a respetiva função e copie a **tecla 'Função'** ou a **tecla 'Anfitrião'.** Saiba mais aqui: [Funções Azure HTTP detona e encadernações](../azure-functions/functions-bindings-http-webhook-trigger.md#authorization-keys) | sim |
|   |   |   |

## <a name="next-steps"></a>Próximos passos

Para obter uma lista das atividades de transformação apoiadas pela Azure Data Factory, consulte [os dados da Transform](transform-data.md).
