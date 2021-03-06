---
title: Transmitir em fluxo registos de aplicação do Azure Spring Cloud em tempo real
description: Como usar o streaming de registo para visualizar registos de aplicações instantaneamente
author: MikeDodaro
ms.author: barbkess
ms.service: spring-cloud
ms.topic: how-to
ms.date: 01/14/2019
ms.custom: devx-track-java, devx-track-azurecli
ms.openlocfilehash: 1eeb291c7a058efd8905e95ebf1ea14fed046691
ms.sourcegitcommit: 32e0fedb80b5a5ed0d2336cea18c3ec3b5015ca1
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/30/2021
ms.locfileid: "104878289"
---
# <a name="stream-azure-spring-cloud-app-logs-in-real-time"></a>Transmitir em fluxo registos de aplicação do Azure Spring Cloud em tempo real

**Este artigo aplica-se a:** ✔️ Java ✔️ C #

O Azure Spring Cloud permite o streaming de registos no Azure CLI para obter registos de consolas de aplicações em tempo real para resolução de problemas. Também pode [analisar registos e métricas com definições de diagnóstico](./diagnostic-services.md).

## <a name="prerequisites"></a>Pré-requisitos

* Instale [a extensão Azure CLI](/cli/azure/install-azure-cli) para a Cloud de primavera, versão mínima 0.2.0 .
* Um exemplo de **Azure Spring Cloud** com uma aplicação em execução, por exemplo, [app Spring Cloud](./spring-cloud-quickstart.md).

> [!NOTE]
>  A extensão ASC CLI é atualizada da versão 0.2.0 para 0.2.1. Esta alteração afeta a sintaxe do comando para o streaming de registos: `az spring-cloud app log tail` , que é substituída por: `az spring-cloud app logs` . O comando: `az spring-cloud app log tail` será depreciado numa futura libertação. Se tiver usado a versão 0.2.0, pode fazer upgrade para 0.2.1. Primeiro, remova a versão antiga com o comando: `az extension remove -n spring-cloud` .  Em seguida, instale 0.2.1 pelo comando: `az extension add -n spring-cloud` .

## <a name="use-cli-to-tail-logs"></a>Use CLI para cauda de troncos

Para evitar especificar repetidamente o nome do seu grupo de recursos e da sua instância de serviço, desaprote o nome do grupo de recursos predefinido e o nome do cluster.
```azurecli
az configure --defaults group=<service group name>
az configure --defaults spring-cloud=<service instance name>
```
Seguindo exemplos, o grupo de recursos e o nome de serviço serão omitidos nos comandos.

### <a name="tail-log-for-app-with-single-instance"></a>Log de cauda para app com instância única
Se uma aplicação chamada auth-service tiver apenas uma instância, pode ver o registo da instância da aplicação com o seguinte comando:
```azurecli
az spring-cloud app logs -n auth-service
```
Isto irá devolver os registos:
```output
...
2020-01-15 01:54:40.481  INFO [auth-service,,,] 1 --- [main] o.apache.catalina.core.StandardService  : Starting service [Tomcat]
2020-01-15 01:54:40.482  INFO [auth-service,,,] 1 --- [main] org.apache.catalina.core.StandardEngine  : Starting Servlet engine: [Apache Tomcat/9.0.22]
2020-01-15 01:54:40.760  INFO [auth-service,,,] 1 --- [main] o.a.c.c.C.[Tomcat].[localhost].[/uaa]  : Initializing Spring embedded WebApplicationContext
2020-01-15 01:54:40.760  INFO [auth-service,,,] 1 --- [main] o.s.web.context.ContextLoader  : Root WebApplicationContext: initialization completed in 7203 ms

...
```

### <a name="tail-log-for-app-with-multiple-instances"></a>Log de cauda para app com várias instâncias
Se existirem várias instâncias para a aplicação `auth-service` nomeada, pode visualizar o registo de ocorrências utilizando a `-i/--instance` opção. 

Em primeiro lugar, pode obter os nomes das instâncias da aplicação com o seguinte comando.

```azurecli
az spring-cloud app show -n auth-service --query properties.activeDeployment.properties.instances -o table
```
Com resultados:

```output
Name                                         Status    DiscoveryStatus
-------------------------------------------  --------  -----------------
auth-service-default-12-75cc4577fc-pw7hb  Running   UP
auth-service-default-12-75cc4577fc-8nt4m  Running   UP
auth-service-default-12-75cc4577fc-n25mh  Running   UP
``` 
Em seguida, pode transmitir registos de uma instância de aplicação com a `-i/--instance` opção:

```azurecli
az spring-cloud app logs -n auth-service -i auth-service-default-12-75cc4577fc-pw7hb
```

Também pode obter detalhes de casos de aplicações a partir do portal Azure.  Depois de selecionar **Apps** no painel de navegação esquerdo do seu serviço Azure Spring Cloud, selecione **Instâncias de Aplicações**.

### <a name="continuously-stream-new-logs"></a>Fluxo contínuo novos troncos
Por predefinição, `az spring-cloud ap log tail` as impressões apenas os registos existentes são transmitidos para a consola de aplicações e depois saem. Se quiser transmitir novos registos, adicione -f (-siga):  

```azurecli
az spring-cloud app logs -n auth-service -f
``` 
Para verificar todas as opções de registo suportados:
```azurecli
az spring-cloud app logs -h 
```

## <a name="next-steps"></a>Passos seguintes
* [Quickstart: Monitorização de aplicativos Azure Spring Cloud com registos, métricas e rastreio](spring-cloud-quickstart-logs-metrics-tracing.md)
* [Analisar registos e métricas com definições de diagnóstico](./diagnostic-services.md)

