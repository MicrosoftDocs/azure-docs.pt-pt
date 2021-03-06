---
title: Recursos apoiados para alertas métricos no Azure Monitor
description: Referência sobre métricas de suporte e registos para alertas métricos no Azure Monitor
author: harelbr
ms.author: harelbr
services: monitoring
ms.topic: conceptual
ms.date: 04/08/2021
ms.openlocfilehash: 24298734a46b8339a2a8818692641b4c10812294
ms.sourcegitcommit: 5f482220a6d994c33c7920f4e4d67d2a450f7f08
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/08/2021
ms.locfileid: "107104886"
---
# <a name="supported-resources-for-metric-alerts-in-azure-monitor"></a>Recursos apoiados para alertas métricos no Azure Monitor

O Azure Monitor suporta agora um [novo tipo de alerta métrico](./alerts-overview.md) que tem benefícios significativos em relação aos mais [antigos alertas métricos clássicos.](./alerts-classic.overview.md) As métricas estão disponíveis para [uma grande lista de serviços Azure.](../essentials/metrics-supported.md) Os alertas mais recentes suportam um subconjunto (crescente) dos tipos de recursos. Este artigo lista o subconjunto.

Também pode utilizar alertas métricos mais recentes em dados de registo populares armazenados num espaço de trabalho log analytics extraído como métricas. Para mais informações, consulte [Alertas métricos para Registos](./alerts-metric-logs.md).

## <a name="portal-powershell-cli-rest-support"></a>Suporte portal, PowerShell, CLI, REST
Atualmente, pode criar alertas métricos mais recentes apenas no portal Azure, [REST API,](/rest/api/monitor/metricalerts/)ou [Modelos de Gestor de Recursos.](./alerts-metric-create-templates.md) O suporte para configurar alertas mais recentes utilizando as versões PowerShell e Azure CLI 2.0 e superiores está para breve.

## <a name="metrics-and-dimensions-supported"></a>Métricas e Dimensões Suportadas
Mais recentes alertas métricos alertam para métricas que usam dimensões. Pode utilizar as dimensões para filtrar a sua métrica para o nível certo. Todas as métricas suportadas juntamente com as dimensões aplicáveis podem ser exploradas e visualizadas a partir do [Azure Monitor - Metrics Explorer](../essentials/metrics-charts.md).

Aqui está a lista completa de fontes métricas do Azure Monitor apoiadas pelos alertas mais recentes:

|Tipo de recurso  |Dimensões Suportadas |Alertas multi-recursos| Métricas disponíveis|
|---------|---------|-----|----------|
|Microsoft.Aadiam/azureADMetrics | Yes | No | [Azure AD](../essentials/metrics-supported.md#microsoftaadiamazureadmetrics) |
|Microsoft.ApiManagement/service | Yes | No | [Gestão de API](../essentials/metrics-supported.md#microsoftapimanagementservice) |
|Microsoft.AppConfiguration/configurationStores |Yes | No | [App Configuration](../essentials/metrics-supported.md#microsoftappconfigurationconfigurationstores) |
|Microsoft.AppPlatform/primavera | Yes | No | [Azure Spring Cloud](../essentials/metrics-supported.md#microsoftappplatformspring) |
|Microsoft.Automation/automation | Yes| No | [Contas de Automatização](../essentials/metrics-supported.md#microsoftautomationautomationaccounts) |
|Microsoft.AVS/privateClouds | No | No | [Solução VMware no Azure](../essentials/metrics-supported.md#microsoftavsprivateclouds) |
|Microsoft.Batcontas ch/batch | Yes | No | [Contas do Batch](../essentials/metrics-supported.md#microsoftbatchbatchaccounts) |
|Microsoft.BotService/botServices | Yes | No | [Serviços bot](../essentials/metrics-supported.md#microsoftbotservicebotservices) |
|Microsoft.Cache/redis | Yes | Yes | [Cache do Azure para Redis](../essentials/metrics-supported.md#microsoftcacheredis) |
|microsoft. Cdn/perfis | Yes | No | [Perfis CDN](../essentials/metrics-supported.md#microsoftcdnprofiles) |
|Microsoft.ClassicCompute/domainNames/slots/roles | No | No | [Serviços clássicos da nuvem](../essentials/metrics-supported.md#microsoftclassiccomputedomainnamesslotsroles) |
|Microsoft.ClassicCompute/virtualMachines | No | No | [Máquinas virtuais clássicas](../essentials/metrics-supported.md#microsoftclassiccomputevirtualmachines) |
|Microsoft.ClassicStorage/storageAcontas | Yes | No | [Contas de Armazenamento (clássicas)](../essentials/metrics-supported.md#microsoftclassicstoragestorageaccounts) |
|Microsoft.ClassicStorage/storageAccounts/blobServices | Yes | No | [Contas de Armazenamento (clássicas) - Blobs](../essentials/metrics-supported.md#microsoftclassicstoragestorageaccountsblobservices) |
|Microsoft.ClassicStorage/storageAcons/fileServices | Yes | No | [Contas de Armazenamento (clássica) - Ficheiros](../essentials/metrics-supported.md#microsoftclassicstoragestorageaccountsfileservices) |
|Microsoft.ClassicStorage/storageAccounts/queueServices | Yes | No | [Contas de Armazenamento (clássica) - Filas](../essentials/metrics-supported.md#microsoftclassicstoragestorageaccountsqueueservices) |
|Microsoft.ClassicStorage/storageAccounts/tableServices | Yes | No | [Contas de Armazenamento (clássica) - Tabelas](../essentials/metrics-supported.md#microsoftclassicstoragestorageaccountstableservices) |
|Microsoft.CognitiveServices/contas | Yes | No | [Serviços Cognitivos](../essentials/metrics-supported.md#microsoftcognitiveservicesaccounts) |
|Microsoft.Compute/cloudServices | Yes | No |  [Serviços em Nuvem](../essentials/metrics-supported.md#microsoftcomputecloudservices) |
|Microsoft.Compute/cloudServices/roles | Yes | No |  [Funções de serviço em nuvem](../essentials/metrics-supported.md#microsoftcomputecloudservicesroles) |
|Microsoft.Compute/virtualMachines | Yes | Sim<sup>1</sup> | [Máquinas Virtuais](../essentials/metrics-supported.md#microsoftcomputevirtualmachines) |
|Microsoft.Compute/virtualMachineScaleSets | Yes | No |[Conjuntos de Dimensionamento de Máquinas Virtuais](../essentials/metrics-supported.md#microsoftcomputevirtualmachinescalesets) |
|Microsoft.ContainerInstance/containerGroups | Yes| No | [Grupos de Contentores](../essentials/metrics-supported.md#microsoftcontainerinstancecontainergroups) |
|Microsoft.ContainerRegistry/registries | No | No | [Registos de Contentores](../essentials/metrics-supported.md#microsoftcontainerregistryregistries) |
|Microsoft.ContainerService/managedClusters | Yes | No | [Clusters Geridos](../essentials/metrics-supported.md#microsoftcontainerservicemanagedclusters) |
|Microsoft.DataBoxEdge/dataBoxEdgeDevices | Yes | Yes | [Data Box](../essentials/metrics-supported.md#microsoftdataboxedgedataboxedgedevices) |
|Microsoft.DataFactory/datafactories| Yes| No | [Fábricas de Dados V1](../essentials/metrics-supported.md#microsoftdatafactorydatafactories) |
|Microsoft.DataFactory/fábricas |Yes | No | [Fábricas de Dados V2](../essentials/metrics-supported.md#microsoftdatafactoryfactories) |
|Microsoft.DataShare/contas | Yes | No | [Ações de Dados](../essentials/metrics-supported.md#microsoftdatashareaccounts) |
|Microsoft.DBforMariaDB/servidores | No | No | [DB para MariaDB](../essentials/metrics-supported.md#microsoftdbformariadbservers) |
|Microsoft.DBforMySQL/servidores | No | No |[DB para MySQL](../essentials/metrics-supported.md#microsoftdbformysqlservers)|
|Microsoft.DBforPostgreSQL/flexibleServers | Yes | No | [DB para PostgreSQL (servidores flexíveis)](../essentials/metrics-supported.md#microsoftdbforpostgresqlflexibleservers)|
|Microsoft.DBforPostgreSQL/serverGroupsv2 | Yes | No | DB para PostgreSQL (hiperescala) |
|Microsoft.DBforPostgreSQL/servidores | No | No | [DB para PostgreSQL](../essentials/metrics-supported.md#microsoftdbforpostgresqlservers)|
|Microsoft.DBforPostgreSQL/serversv2 | No | No | [DB para PostgreSQL V2](../essentials/metrics-supported.md#microsoftdbforpostgresqlserversv2)|
|Microsoft.Devices/IotHubs | Yes | No |[Hub IoT](../essentials/metrics-supported.md#microsoftdevicesiothubs) |
|Microsoft.Devices/provisioningServices| Yes | No | [Serviços de prestação de dispositivos](../essentials/metrics-supported.md#microsoftdevicesprovisioningservices) |
|Microsoft.DigitalTwins/digitalTwinsInstances | Yes | No | [Digital Twins](../essentials/metrics-supported.md#microsoftdigitaltwinsdigitaltwinsinstances) |
|Microsoft.DocumentDB/base de dadosAcontas | Yes | No | [BD do Cosmos](../essentials/metrics-supported.md#microsoftdocumentdbdatabaseaccounts) |
|Microsoft.EventGrid/domínios | Yes | No | [Domínios do Event Grid](../essentials/metrics-supported.md#microsofteventgriddomains) |
|Microsoft.EventGrid/systemTopics | Yes | No | [Tópicos do sistema de grelha de eventos](../essentials/metrics-supported.md#microsofteventgridsystemtopics) |
|Microsoft.EventGrid/tópicos |Yes | No | [Tópicos do Event Grid](../essentials/metrics-supported.md#microsofteventgridtopics) |
|Microsoft.EventHub/clusters |Yes| No | [Clusters hubs de eventos](../essentials/metrics-supported.md#microsofteventhubclusters) |
|Microsoft.EventHub/espaços de nome |Yes| No | [Hubs de Eventos](../essentials/metrics-supported.md#microsofteventhubnamespaces) |
|Microsoft.HDInsight/clusters | Yes | No | [Clusters do HDInsight](../essentials/metrics-supported.md#microsofthdinsightclusters) |
|Microsoft.Insights/Componentes | Yes | No | [Application Insights](../essentials/metrics-supported.md#microsoftinsightscomponents) |
|Microsoft.KeyVault/cofres | Yes |Yes |[Cofres](../essentials/metrics-supported.md#microsoftkeyvaultvaults)|
|Microsoft.Kusto/Clusters | Yes |No |[Clusters de Exploradores de Dados](../essentials/metrics-supported.md#microsoftkustoclusters)|
|Microsoft.Logic/integrationServiceEnvironments | Yes | No |[Ambientes de Serviço de Integração](../essentials/metrics-supported.md#microsoftlogicintegrationserviceenvironments) |
|Microsoft.Logic/workflows | No | No |[Logic Apps](../essentials/metrics-supported.md#microsoftlogicworkflows) |
|Microsoft.MachineLearningServices/workspaces | Yes | No | [Machine Learning](../essentials/metrics-supported.md#microsoftmachinelearningservicesworkspaces) |
|Microsoft.MachineLearningServices/workspaces/onlineEndpoints | Yes | No | Machine Learning - Pontos finais |
|Microsoft.MachineLearningServices/workspaces/onlineEndpoints/deployments | Yes | No | Machine Learning - Endpoint Deployments |
|Microsoft.Maps/contas | Yes | No | [Contas de Mapas](../essentials/metrics-supported.md#microsoftmapsaccounts) |
|Microsoft.Media/mediaservices | No | No | [Serviços de Multimédia](../essentials/metrics-supported.md#microsoftmediamediaservices) |
|Microsoft.Media/mediaservices/streamingEndpoints | Yes | No | [Pontos finais de streaming de serviços de mídia](../essentials/metrics-supported.md#microsoftmediamediaservicesstreamingendpoints) |
|Microsoft.NetApp/netAppAccounts/capacityPools | Yes | Yes | [Piscinas de capacidade Azure NetApp](../essentials/metrics-supported.md#microsoftnetappnetappaccountscapacitypools) |
|Microsoft.NetApp/netAppAccounts/capacityPools/volumes | Yes | Yes | [Azure NetApp Volumes](../essentials/metrics-supported.md#microsoftnetappnetappaccountscapacitypoolsvolumes) |
|Microsoft.Network/applicationGateways | Yes | No | [Gateways de Aplicação](../essentials/metrics-supported.md#microsoftnetworkapplicationgateways) |
|Microsoft.Network/azurefirewalls | Yes | No | [Firewalls](../essentials/metrics-supported.md#microsoftnetworkazurefirewalls) |
|Microsoft.Network/dnsZones | No | No | [Zonas DNS](../essentials/metrics-supported.md#microsoftnetworkdnszones) |
|Microsoft.Network/expressRouteCircuits | Yes | No |[Circuitos ExpressRoute](../essentials/metrics-supported.md#microsoftnetworkexpressroutecircuits) |
|Microsoft.Network/expressRoutePorts | Yes | No |[ExpressRoute Direct](../essentials/metrics-supported.md#microsoftnetworkexpressrouteports) |
|Microsoft.Network/loadBalancers (apenas para SKUs padrão)| Yes| No | [Balançadores de Carga](../essentials/metrics-supported.md#microsoftnetworkloadbalancers) |
|Microsoft.Network/natGateways| No | No | [NAT Gateways](../essentials/metrics-supported.md#microsoftnetworknatgateways) |
|Microsoft.Network/privateEndpoints| No | No | [Pontos finais privados](../essentials/metrics-supported.md#microsoftnetworkprivateendpoints) |
|Microsoft.Network/privateLinkServices| No | No | [Serviços de Ligação Privada](../essentials/metrics-supported.md#microsoftnetworkprivatelinkservices) |
|Microsoft.Network/publicipaddresss | No | No | [Endereços IP públicos](../essentials/metrics-supported.md#microsoftnetworkpublicipaddresses)|
|Microsoft.Network/trafficManagerProfiles | Yes | No | [Perfis do Gestor de Tráfego](../essentials/metrics-supported.md#microsoftnetworktrafficmanagerprofiles) |
|Microsoft.OperationalInsights/workspaces| Yes | No | [Áreas de trabalho do Log Analytics](../essentials/metrics-supported.md#microsoftoperationalinsightsworkspaces)|
|Microsoft.Peering/peerings | Yes | No | [Peerings](../essentials/metrics-supported.md#microsoftpeeringpeerings) |
|Microsoft.Peering/peeringServices | Yes | No | [Peering Services](../essentials/metrics-supported.md#microsoftpeeringpeeringservices) |
|Microsoft.PowerBIDedicated/capacities | No | No | [Capacidades](../essentials/metrics-supported.md#microsoftpowerbidedicatedcapacities) |
|Microsoft.Relay/namespaces | Yes | No | [Reencaminhamentos](../essentials/metrics-supported.md#microsoftrelaynamespaces) |
|Microsoft.Search/searchServices | No | No | [Serviços de pesquisa](../essentials/metrics-supported.md#microsoftsearchsearchservices) |
|Microsoft.ServiceBus/namespaces | Yes | No | [Service Bus](../essentials/metrics-supported.md#microsoftservicebusnamespaces) |
|Microsoft.Sql/managedInstances | No | Yes | [Instâncias Geridas SQL](../essentials/metrics-supported.md#microsoftsqlmanagedinstances) |
|Microsoft.Sql/servidores/bases de dados | No | Yes | [Bases de Dados SQL](../essentials/metrics-supported.md#microsoftsqlserversdatabases) |
|Microsoft.Sql/servidores/elásticos | No | Yes | [Piscinas Elásticas SQL](../essentials/metrics-supported.md#microsoftsqlserverselasticpools) |
|Microsoft.Storage/storageAcontas |Yes | No | [Contas de Armazenamento](../essentials/metrics-supported.md#microsoftstoragestorageaccounts)|
|Microsoft.Storage/storageAcontas/blobServices | Yes| No | [Contas de Armazenamento - Blobs](../essentials/metrics-supported.md#microsoftstoragestorageaccountsblobservices) |
|Microsoft.Storage/storageAcontas/fileServices | Yes| No | [Contas de Armazenamento - Ficheiros](../essentials/metrics-supported.md#microsoftstoragestorageaccountsfileservices) |
|Microsoft.Storage/storageAcontas/filas Serviços | Yes| No | [Contas de Armazenamento - Filas](../essentials/metrics-supported.md#microsoftstoragestorageaccountsqueueservices) |
|Microsoft.Storage/storageAcontas/tableServices | Yes| No | [Contas de Armazenamento - Tabelas](../essentials/metrics-supported.md#microsoftstoragestorageaccountstableservices) |
|Microsoft.StorageCache/caches | Yes | No | [HPC Caches](../essentials/metrics-supported.md#microsoftstoragecachecaches) |
|Microsoft.StorageSync/storageSyncServices | Yes | No | [Serviços de Sincronização de Armazenamento](../essentials/metrics-supported.md#microsoftstoragesyncstoragesyncservices) |
|Microsoft.StreamAnalytics/streamingjobs | Yes | No | [Stream Analytics](../essentials/metrics-supported.md#microsoftstreamanalyticsstreamingjobs) |
|Microsoft.Synapse/workspaces | Yes | No | [Synapse Analytics](../essentials/metrics-supported.md#microsoftsynapseworkspaces) |
|Microsoft.Synapse/workspaces/bigDataPools | Yes | No | [Piscinas de faíscas Apache Spark](../essentials/metrics-supported.md#microsoftsynapseworkspacesbigdatapools) |
|Microsoft.Synapse/workspaces/sqlPools | Yes | No | [Piscinas SQL Synapse Analytics](../essentials/metrics-supported.md#microsoftsynapseworkspacessqlpools) |
|Microsoft.VMWareCloudSimple/virtualMachines | Yes | No | [Máquinas Virtuais do CloudSimple](../essentials/metrics-supported.md#microsoftvmwarecloudsimplevirtualmachines) |
|Microsoft.Web/hostingEnvironments/multiRolePools | Yes | No | [Piscinas multi-role de ambiente de serviço de aplicativos](../essentials/metrics-supported.md#microsoftwebhostingenvironmentsmultirolepools)|
|Microsoft.Web/hostingEnvironments/workerPools | Yes | No | [Piscinas de trabalhadores do ambiente do serviço de aplicações](../essentials/metrics-supported.md#microsoftwebhostingenvironmentsworkerpools)|
|Microsoft.Web/serverfarms | Yes | No | [Planos de Serviço de Aplicações](../essentials/metrics-supported.md#microsoftwebserverfarms)|
|Microsoft.Web/sites | Yes | No | [Serviços e Funções de Aplicativos](../essentials/metrics-supported.md#microsoftwebsites)|
|Microsoft.Web/sites/slots | Yes | No | [Slots do Serviço de Aplicações](../essentials/metrics-supported.md#microsoftwebsitesslots)|

<sup>1</sup> Não suportado para métricas de rede de máquinas virtuais (Rede Total, Rede Fora Total, Fluxos de Entrada, Fluxos de Saída, Fluxos de Entrada Taxa máxima de criação, Taxa máxima de criação de fluxos de saída) e métricas personalizadas.

## <a name="payload-schema"></a>Esquema de carga útil

> [!NOTE]
> Também pode utilizar o [esquema de alerta comum,](./alerts-common-schema.md)que proporciona a vantagem de ter uma única carga de alerta extensível e unificada em todos os serviços de alerta em Azure Monitor, para as suas integrações webhook. [Conheça as definições comuns de esquema de alerta.](./alerts-common-schema-definitions.md)


A operação POST contém a seguinte carga útil e esquema JSON para todos os alertas métricos mais recentes quando é utilizado um [grupo de ação](./action-groups.md) devidamente configurado:

```json
{
  "schemaId": "AzureMonitorMetricAlert",
  "data": {
    "version": "2.0",
    "status": "Activated",
    "context": {
      "timestamp": "2018-02-28T10:44:10.1714014Z",
      "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/Contoso/providers/microsoft.insights/metricAlerts/StorageCheck",
      "name": "StorageCheck",
      "description": "",
      "conditionType": "SingleResourceMultipleMetricCriteria",
      "severity":"3",
      "condition": {
        "windowSize": "PT5M",
        "allOf": [
          {
            "metricName": "Transactions",
            "metricNamespace":"microsoft.storage/storageAccounts",
            "dimensions": [
              {
                "name": "AccountResourceId",
                "value": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/Contoso/providers/Microsoft.Storage/storageAccounts/diag500"
              },
              {
                "name": "GeoType",
                "value": "Primary"
              }
            ],
            "operator": "GreaterThan",
            "threshold": "0",
            "timeAggregation": "PT5M",
            "metricValue": 1
          }
        ]
      },
      "subscriptionId": "00000000-0000-0000-0000-000000000000",
      "resourceGroupName": "Contoso",
      "resourceName": "diag500",
      "resourceType": "Microsoft.Storage/storageAccounts",
      "resourceId": "/subscriptions/1e3ff1c0-771a-4119-a03b-be82a51e232d/resourceGroups/Contoso/providers/Microsoft.Storage/storageAccounts/diag500",
      "portalLink": "https://portal.azure.com/#resource//subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/Contoso/providers/Microsoft.Storage/storageAccounts/diag500"
    },
    "properties": {
      "key1": "value1",
      "key2": "value2"
    }
  }
}
```

## <a name="next-steps"></a>Passos seguintes

* Saiba mais sobre a nova [experiência Alertas.](./alerts-overview.md)
* Saiba mais sobre [os alertas de registo em Azure](./alerts-unified-log.md).
* Conheça os [alertas em Azure.](./alerts-overview.md)
