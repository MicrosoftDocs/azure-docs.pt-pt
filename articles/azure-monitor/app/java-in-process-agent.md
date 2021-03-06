---
title: Azure Monitor Application Insights Java
description: Monitorização do desempenho da aplicação para aplicações Java em qualquer ambiente sem necessidade de modificação de código. Mapa de rastreio e aplicação distribuídos.
ms.topic: conceptual
ms.date: 03/29/2020
author: MS-jgol
ms.custom: devx-track-java
ms.author: jgol
ms.openlocfilehash: 3f22e165fe4a3f86ecce8b1e307b19fae0eeac81
ms.sourcegitcommit: 260a2541e5e0e7327a445e1ee1be3ad20122b37e
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/21/2021
ms.locfileid: "107812053"
---
# <a name="java-codeless-application-monitoring-azure-monitor-application-insights"></a>Java aplicação codificada monitorizando Azure Monitor Application Insights

A monitorização da aplicação sem código java tem tudo a ver com simplicidade - não há alterações de código, o agente Java pode ser ativado através de apenas algumas alterações de configuração.

 O agente Java trabalha em qualquer ambiente, e permite-lhe monitorizar todas as suas aplicações Java. Por outras palavras, quer esteja a executar as suas aplicações Java em VMs, no local, em AKS, no Windows, Linux - diga o que quiser, o agente Java 3.0 irá monitorizar a sua aplicação.

A adição da Aplicação Insights Java SDK à sua aplicação já não é necessária, uma vez que o agente 3.0 recolhe automaticamente pedidos, dependências e registos por si só.

Ainda pode enviar telemetria personalizada a partir da sua aplicação. O agente 3.0 irá rastreá-lo e correlacioná-lo juntamente com toda a telemetria auto-recolhida.

O agente 3.0 suporta Java 8 ou superior.

## <a name="quickstart"></a>Início Rápido

**1. Descarregue o agente**

> [!WARNING]
> **Se está a atualizar a partir de 3.0 Preview**
>
> Por favor, reveja cuidadosamente todas as opções de [configuração,](./java-standalone-config.md) uma vez que a estrutura json mudou completamente, além do próprio nome de ficheiro que foi tudo minúsculo.

Baixar [aplicaçõesinsights-agent-3.0.3.jar](https://github.com/microsoft/ApplicationInsights-Java/releases/download/3.0.3/applicationinsights-agent-3.0.3.jar)

**2. Aponte o JVM ao agente**

Adicione `-javaagent:path/to/applicationinsights-agent-3.0.3.jar` aos args JVM da sua aplicação

Os arg típicos jvm incluem `-Xmx512m` e `-XX:+UseG1GC` . Então, se sabe onde adicionar isto, então já sabe onde adicionar isto.

Para obter ajuda adicional para configurar os args JVM da sua aplicação, consulte [dicas para atualizar os seus args JVM](./java-standalone-arguments.md).

**3. Aponte o agente para o seu recurso Application Insights**

Se ainda não tiver um recurso Application Insights, pode criar um novo seguindo os passos no guia de [criação de recursos.](./create-new-resource.md)

Aponte o agente para o seu recurso Application Insights, quer definindo uma variável ambiental:

```
APPLICATIONINSIGHTS_CONNECTION_STRING=InstrumentationKey=...
```

Ou criando um ficheiro de configuração denominado `applicationinsights.json` , e colocando-o no mesmo diretório `applicationinsights-agent-3.0.3.jar` que, com o seguinte conteúdo:

```json
{
  "connectionString": "InstrumentationKey=..."
}
```

Pode encontrar a sua cadeia de ligação no seu recurso Application Insights:

:::image type="content" source="media/java-ipa/connection-string.png" alt-text="Cadeia de conexão de insights de aplicação":::

**4. É isto!**

Agora inicie a sua aplicação e vá ao seu recurso Application Insights no portal Azure para ver os seus dados de monitorização.

> [!NOTE]
> Pode levar alguns minutos para que os seus dados de monitorização apareçam no portal.


## <a name="configuration-options"></a>Opções de configuração

No `applicationinsights.json` ficheiro, pode ainda configurar:

* Nome do papel da nuvem
* Instância de papel em nuvem
* Amostragem
* Métricas JMX
* Dimensões personalizadas
* Processadores de telemetria (pré-visualização)
* Registos auto-recolhidos
* Métricas de micrometros auto-recolhidas (incluindo métricas do actuador de bota de mola)
* Heartbeat
* HTTP Proxy
* Autodiagnóssia

Consulte [as opções de configuração](./java-standalone-config.md) para obter todos os detalhes.

## <a name="auto-collected-requests-dependencies-logs-and-metrics"></a>Pedidos, dependências, registos e métricas recolhidos automaticamente

### <a name="requests"></a>Pedidos

* Consumidores JMS
* Consumidores kafka
* Netty/WebFlux
* Servlets
* Agendamento da primavera

### <a name="dependencies-with-distributed-trace-propagation"></a>Dependências com propagação de vestígios distribuídos

* Apache HttpClient e HttpAsyncClient
* gRPC
* java.net.HttpURLConnection
* JMS
* Kafka
* Cliente Netty
* OkHttp

### <a name="other-dependencies"></a>Outras dependências

* Cassandra
* JDBC
* MongoDB (assíco e sincronização)
* Redis (Alface e Jedis)

### <a name="logs"></a>Registos

* java.util.logging
* Log4j (incluindo propriedades MDC)
* SLF4J/Logback (incluindo propriedades MDC)

### <a name="metrics"></a>Métricas

* Micrometro (incluindo métricas do actuador de bota de mola)
* Métricas JMX

### <a name="azure-sdks-preview"></a>Azure SDKs (pré-visualização)

Consulte as [opções de configuração](./java-standalone-config.md#auto-collected-azure-sdk-telemetry-preview) para ativar esta funcionalidade de pré-visualização e capture a telemetria emitida por estes Azure SDKs:

* [Configuração da aplicação](https://docs.microsoft.com/java/api/overview/azure/data-appconfiguration-readme) 1.1.10+
* [Pesquisa Cognitiva](https://docs.microsoft.com/java/api/overview/azure/search-documents-readme) 11.3.0+
* [Chat de Comunicação](https://docs.microsoft.com/java/api/overview/azure/communication-chat-readme) 1.0.0+
* [Comunicação Comum](https://docs.microsoft.com/java/api/overview/azure/communication-common-readme) 1.0.0+
* [Identidade de Comunicação](https://docs.microsoft.com/java/api/overview/azure/communication-identity-readme) 1.0.0+
* [Sms de comunicação](https://docs.microsoft.com/java/api/overview/azure/communication-sms-readme) 1.0.0+
* [Cosmos DB](https://docs.microsoft.com/java/api/overview/azure/cosmos-readme) 4.13.0+
* [Grelha de Eventos](https://docs.microsoft.com/java/api/overview/azure/messaging-eventgrid-readme) 4.0.0+
* [Centros de Eventos](https://docs.microsoft.com/java/api/overview/azure/messaging-eventhubs-readme) 5.6.0+
* [Centros de Eventos - Azure Blob Storage Checkpoint Store](https://docs.microsoft.com/java/api/overview/azure/messaging-eventhubs-checkpointstore-blob-readme) 1.5.1+
* [Reconhecimento de Formulários](https://docs.microsoft.com/java/api/overview/azure/ai-formrecognizer-readme) 3.0.6+
* [Identidade](https://docs.microsoft.com/java/api/overview/azure/identity-readme) 1.2.4+
* [Cofre-chave - Certificados](https://docs.microsoft.com/java/api/overview/azure/security-keyvault-certificates-readme) 4.1.6+
* [Cofre chave - Chaves](https://docs.microsoft.com/java/api/overview/azure/security-keyvault-keys-readme) 4.2.6+
* [Cofre chave - Segredos](https://docs.microsoft.com/java/api/overview/azure/security-keyvault-secrets-readme) 4.2.6+
* [Ônibus de serviço](https://docs.microsoft.com/java/api/overview/azure/messaging-servicebus-readme) 7.1.0+
* [Análise de Texto](https://docs.microsoft.com/java/api/overview/azure/ai-textanalytics-readme) 5.0.4+

[//]: # "os nomes e links acima raspados de https://azure.github.io/azure-sdk/releases/latest/java.html"
[//]: # "e versão sincronizada manualmente contra a versão mais antiga em maven central construída em azure-core 1.14.0"
[//]: # ""
[//]: # "var tabela = document.consultaSelector ('#tg-sb-content > div > tabela')"
[//]: # "var str = ''"
[//]: # "para (var i = 1, linha; linha = mesa.linhas[i]; i++) {"
[//]: # "  nome var = linha.cells[0].getElementsByTagName ('div')[0].textContent.trim()"
[//]: # "  var stableRow = linha.cells[1]"
[//]: # "  versão varBadge = stableRow.consultaSelector ('.badge')"
[//]: # "  se (!versãoBadge) {"
[//]: # "    continuar"
[//]: # "  }"
[//]: # "  versão var = versãoBadge.textContent.trim()"
[//]: # "  var link = stableRow.consultaSelectorAll('a')[2].href"
[//]: # "  str += '* [' + nome + ''(' + link + ') + versão"
[//]: # "}"
[//]: # "consola.log(str)"

## <a name="send-custom-telemetry-from-your-application"></a>Envie telemetria personalizada a partir da sua aplicação

O nosso objetivo em 3.0+ é permitir-lhe enviar a sua telemetria personalizada usando APIs padrão.

Apoiamos micrometros, quadros de registo populares, e a Aplicação Insights Java 2.x SDK até agora.
Application Insights Java 3.0 captura automaticamente a telemetria enviada através destas APIs e correlaciona-a com telemetria recolhida automaticamente.

### <a name="supported-custom-telemetry"></a>Telemetria personalizada suportada

O quadro abaixo representa os tipos de telemetria personalizados atualmente suportados que pode permitir complementar o agente Java 3.0. Resumindo, as métricas personalizadas são suportadas através de micrometros, as exceções personalizadas e os vestígios podem ser ativados através de quadros de registo, e qualquer tipo de telemetria personalizada é suportada através do [Application Insights Java 2.x SDK](#send-custom-telemetry-using-the-2x-sdk).

|                     | Micrometer | Log4j, logback, JUL | 2.x SDK |
|---------------------|------------|---------------------|---------|
| **Eventos Personalizados**   |            |                     |  Yes    |
| **Métricas Personalizadas**  |  Yes       |                     |  Yes    |
| **Dependências**    |            |                     |  Yes    |
| **Exceções**      |            |  Yes                |  Yes    |
| **Vistas de página**      |            |                     |  Yes    |
| **Pedidos**        |            |                     |  Yes    |
| **Rastreios**          |            |  Yes                |  Yes    |

Não estamos a planear lançar um SDK com o Application Insights 3.0 neste momento.

Application Insights Java 3.0 já está a ouvir a telemetria que é enviada para a Aplicação Insights Java 2.x SDK. Esta funcionalidade é uma parte importante da história de upgrade para os utilizadores existentes em 2.x, e preenche uma lacuna importante no nosso suporte de telemetria personalizado até que a API OpenTelemetry seja GA.

### <a name="send-custom-metrics-using-micrometer"></a>Envie métricas personalizadas usando o Micrometro

Adicione micrometro à sua aplicação:

```xml
<dependency>
  <groupId>io.micrometer</groupId>
  <artifactId>micrometer-core</artifactId>
  <version>1.6.1</version>
</dependency>
```

Utilize o [registo global](https://micrometer.io/docs/concepts#_global_registry) do Micrometro para criar um medidor:

```java
static final Counter counter = Metrics.counter("test_counter");
```

e usá-lo para registar métricas:

```java
counter.increment();
```

### <a name="send-custom-traces-and-exceptions-using-your-favorite-logging-framework"></a>Envie vestígios e exceções personalizados usando a sua estrutura de registo favorito

Log4j, Logback e java.util.logging são instrumentados automaticamente, e o registo realizado através destas estruturas de registo é recolhido automaticamente como vestígio e telemetria de exceção.

Por predefinição, a exploração madeireira só é recolhida quando essa sessão é realizada ao nível info ou acima.
Consulte as opções de [configuração](./java-standalone-config.md#auto-collected-logging) para como alterar este nível.

Se pretender anexar dimensões personalizadas aos seus registos, pode utilizar [o Log4j 1.2 MDC,](https://logging.apache.org/log4j/1.2/apidocs/org/apache/log4j/MDC.html) [o Log4j 2 MDC,](https://logging.apache.org/log4j/2.x/manual/thread-context.html)ou [o Logback MDC,](http://logback.qos.ch/manual/mdc.html)e o Application Insights Java 3.0 capturará automaticamente essas propriedades MDC como dimensões personalizadas no seu vestígio e telemetria de exceção.

### <a name="send-custom-telemetry-using-the-2x-sdk"></a>Envie telemetria personalizada usando o 2.x SDK

Adicione `applicationinsights-core-2.6.2.jar` à sua aplicação (todas as versões 2.x são suportadas pela Application Insights Java 3.0, mas vale a pena usar as últimas se tiver escolha):

```xml
<dependency>
  <groupId>com.microsoft.azure</groupId>
  <artifactId>applicationinsights-core</artifactId>
  <version>2.6.2</version>
</dependency>
```

Criar um TelemetriaClient:

  ```java
static final TelemetryClient telemetryClient = new TelemetryClient();
```

e usá-lo para enviar telemetria personalizada:

##### <a name="events"></a>Eventos

```java
telemetryClient.trackEvent("WinGame");
```

##### <a name="metrics"></a>Métricas

```java
telemetryClient.trackMetric("queueLength", 42.0);
```

##### <a name="dependencies"></a>Dependências

```java
boolean success = false;
long startTime = System.currentTimeMillis();
try {
    success = dependency.call();
} finally {
    long endTime = System.currentTimeMillis();
    RemoteDependencyTelemetry telemetry = new RemoteDependencyTelemetry();
    telemetry.setSuccess(success);
    telemetry.setTimestamp(new Date(startTime));
    telemetry.setDuration(new Duration(endTime - startTime));
    telemetryClient.trackDependency(telemetry);
}
```

##### <a name="logs"></a>Registos

```java
telemetryClient.trackTrace(message, SeverityLevel.Warning, properties);
```

##### <a name="exceptions"></a>Exceções

```java
try {
    ...
} catch (Exception e) {
    telemetryClient.trackException(e);
}
```

### <a name="add-request-custom-dimensions-using-the-2x-sdk"></a>Adicione as dimensões personalizadas do pedido usando o 2.x SDK

> [!NOTE]
> Esta característica é apenas em 3.0.2 e mais tarde

Adicione `applicationinsights-web-2.6.2.jar` à sua aplicação (todas as versões 2.x são suportadas pela Application Insights Java 3.0, mas vale a pena usar as últimas se tiver escolha):

```xml
<dependency>
  <groupId>com.microsoft.azure</groupId>
  <artifactId>applicationinsights-web</artifactId>
  <version>2.6.2</version>
</dependency>
```

e adicionar dimensões personalizadas no seu código:

```java
import com.microsoft.applicationinsights.web.internal.ThreadContext;

RequestTelemetry requestTelemetry = ThreadContext.getRequestTelemetryContext().getHttpRequestTelemetry();
requestTelemetry.getProperties().put("mydimension", "myvalue");
```

### <a name="set-the-request-telemetry-user_id-using-the-2x-sdk"></a>Desaponte a telemetria de pedido user_Id utilizando o 2.x SDK

> [!NOTE]
> Esta característica é apenas em 3.0.2 e mais tarde

Adicione `applicationinsights-web-2.6.2.jar` à sua aplicação (todas as versões 2.x são suportadas pela Application Insights Java 3.0, mas vale a pena usar as últimas se tiver escolha):

```xml
<dependency>
  <groupId>com.microsoft.azure</groupId>
  <artifactId>applicationinsights-web</artifactId>
  <version>2.6.2</version>
</dependency>
```

e definir o `user_Id` no seu código:

```java
import com.microsoft.applicationinsights.web.internal.ThreadContext;

RequestTelemetry requestTelemetry = ThreadContext.getRequestTelemetryContext().getHttpRequestTelemetry();
requestTelemetry.getContext().getUser().setId("myuser");
```

### <a name="override-the-request-telemetry-name-using-the-2x-sdk"></a>Anular o nome de telemetria do pedido utilizando o 2.x SDK

> [!NOTE]
> Esta característica é apenas em 3.0.2 e mais tarde

Adicione `applicationinsights-web-2.6.2.jar` à sua aplicação (todas as versões 2.x são suportadas pela Application Insights Java 3.0, mas vale a pena usar as últimas se tiver escolha):

```xml
<dependency>
  <groupId>com.microsoft.azure</groupId>
  <artifactId>applicationinsights-web</artifactId>
  <version>2.6.2</version>
</dependency>
```

e definir o nome no seu código:

```java
import com.microsoft.applicationinsights.web.internal.ThreadContext;

RequestTelemetry requestTelemetry = ThreadContext.getRequestTelemetryContext().getHttpRequestTelemetry();
requestTelemetry.setName("myname");
```

### <a name="get-the-request-telemetry-id-and-the-operation-id-using-the-2x-sdk"></a>Obtenha o id de telemetria de pedido e o id de operação usando o 2.x SDK

> [!NOTE]
> Esta característica é apenas em 3.0.3 e mais tarde

Adicione `applicationinsights-web-2.6.2.jar` à sua aplicação (todas as versões 2.x são suportadas pela Application Insights Java 3.0, mas vale a pena usar as últimas se tiver escolha):

```xml
<dependency>
  <groupId>com.microsoft.azure</groupId>
  <artifactId>applicationinsights-web</artifactId>
  <version>2.6.2</version>
</dependency>
```

e obter o pedido de telemetria id e o id de operação no seu código:

```java
import com.microsoft.applicationinsights.web.internal.ThreadContext;

RequestTelemetry requestTelemetry = ThreadContext.getRequestTelemetryContext().getHttpRequestTelemetry();
String requestId = requestTelemetry.getId();
String operationId = requestTelemetry.getContext().getOperation().getId();
```
