---
title: Desenvolver funções de biblioteca de classes C# usando Funções Azure
description: Entenda como usar o C# para desenvolver e publicar código como bibliotecas de classe que funciona em processo com o tempo de funcionamento das Funções Azure.
ms.topic: conceptual
ms.custom: devx-track-csharp
ms.date: 07/24/2020
ms.openlocfilehash: c7d14599ec1ebbcb94e0c0f3985a3b857f9353dc
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/30/2021
ms.locfileid: "102563885"
---
# <a name="develop-c-class-library-functions-using-azure-functions"></a>Desenvolver funções de biblioteca de classes C# usando Funções Azure

<!-- When updating this article, make corresponding changes to any duplicate content in functions-reference-csharp.md -->

Este artigo é uma introdução ao desenvolvimento de Funções Azure utilizando C# em bibliotecas de classe .NET.

>[!IMPORTANT]
>Este artigo suporta funções de biblioteca de classe .NET que funcionam em processo com o tempo de funcionamento. As funções também suportam .NET 5.x executando as suas funções C# fora do processo e isoladas do tempo de funcionamento. Para saber mais, consulte [as funções de processo isoladas .NET](dotnet-isolated-process-guide.md).

Como desenvolvedor C#, também poderá estar interessado num dos seguintes artigos:

| Introdução | Conceitos| Aprendizagem/amostras guiadas |
|--| -- |--| 
| <ul><li>[Com o Visual Studio](functions-create-your-first-function-visual-studio.md)</li><li>[Utilizar o Visual Studio Code](create-first-function-vs-code-csharp.md)</li><li>[Usando ferramentas de linha de comando](create-first-function-cli-csharp.md)</li></ul> | <ul><li>[Opções de alojamento](functions-scale.md)</li><li>[&nbsp;Considerações de desempenho](functions-best-practices.md)</li><li>[Desenvolvimento do Visual Studio](functions-develop-vs.md)</li><li>[Injeção de dependência](functions-dotnet-dependency-injection.md)</li></ul> | <ul><li>[Criar aplicações sem servidor](/learn/paths/create-serverless-applications/)</li><li>[Amostras em C#](/samples/browse/?products=azure-functions&languages=csharp)</li></ul> |

A Azure Functions suporta linguagens de programação de scripts C# e C#. Se estiver à procura de orientação sobre [a utilização de C# no portal Azure,](functions-create-function-app-portal.md)consulte a [referência do programador do script C# (.csx).](functions-reference-csharp.md)

## <a name="supported-versions"></a>Versões suportadas

As versões do tempo de execução das Funções funcionam com versões específicas de .NET. Para saber mais sobre as versões de Funções, consulte [as versões de tempo de execução do Azure Functions](functions-versions.md)

A tabela a seguir mostra o nível mais alto de .NET Core ou .NET Framework que pode ser usado com uma versão específica de Funções. 

| Versão de tempo de execução de funções | Versão Max .NET |
| ---- | ---- |
| Funções 3.x | .NET Core 3.1<br/>.NET 5.0<sup>1</sup> |
| Funções 2.x | .NET Core 2.2<sup>2</sup> |
| Funções 1.x | .Net Quadro 4.7 |

<sup>1</sup> Deve [ficar sem processo.](dotnet-isolated-process-guide.md)  
<sup>2</sup> Para mais detalhes, consulte [as considerações de Funções v2.x](#functions-v2x-considerations).   

Para as últimas notícias sobre os lançamentos do Azure Functions, incluindo a remoção de versões menores específicas, monitorize [os anúncios do Azure App Service](https://github.com/Azure/app-service-announcements/issues).

### <a name="functions-v2x-considerations"></a>Funções v2.x considerações

As aplicações de função que visam a versão mais recente 2.x ( `~2` ) são automaticamente atualizadas para serem executadas em .NET Core 3.1. Devido à quebra de alterações entre as versões .NET Core, nem todas as aplicações desenvolvidas e compiladas contra .NET Core 2.2 podem ser atualizadas com segurança para .NET Core 3.1. Pode optar por não fazer esta atualização fixando a sua aplicação de função a `~2.0` . As funções também detetam APIs incompatíveis e podem fixar a sua aplicação `~2.0` para evitar uma execução incorreta em .NET Core 3.1. 

>[!NOTE]
>Se a sua aplicação de função estiver presa `~2.0` e alterar este alvo de versão `~2` para, a sua aplicação de função poderá quebrar. Se implementar usando modelos ARM, verifique a versão nos seus modelos. Se isto ocorrer, altere a sua versão de volta para target `~2.0` e corrija problemas de compatibilidade. 

As aplicações de função que visam `~2.0` continuam a funcionar em .NET Core 2.2. Esta versão de .NET Core já não recebe atualizações de segurança e outras atualizações de manutenção. Para saber mais, consulte [esta página de anúncios.](https://github.com/Azure/app-service-announcements/issues/266) 

Deve trabalhar para tornar as suas funções compatíveis com .NET Core 3.1 o mais rapidamente possível. Depois de resolver estes problemas, altere a sua versão de volta `~2` para `~3` . Para saber mais sobre versões de segmentação do tempo de execução das Funções, consulte [como direcionar as versões de tempo de execução do Azure Functions](set-runtime-version.md).

Ao executar o Linux num plano Premium ou dedicado (Serviço de Aplicações), fixa a sua versão, ao invés de direcionar uma imagem específica, definindo a `linuxFxVersion` definição de configuração do site `DOCKER|mcr.microsoft.com/azure-functions/dotnet:2.0.14786-appservice` para aprender a definir , consulte `linuxFxVersion` [atualizações de versão manual no Linux](set-runtime-version.md#manual-version-updates-on-linux).

## <a name="functions-class-library-project"></a>Projeto de biblioteca de classes de funções

No Visual Studio, o modelo de projeto **Azure Functions** cria um projeto de biblioteca de classe C# que contém os seguintes ficheiros:

* [host.jsligado](functions-host-json.md) - armazena configurações de configuração que afetam todas as funções do projeto quando funciona localmente ou em Azure.
* [local.settings.js-](functions-run-local.md#local-settings-file) armazena as definições de aplicações e as cordas de ligação que são usadas quando são executando localmente. Este ficheiro contém segredos e não foi publicado na sua aplicação de função em Azure. Em vez disso, [adicione as definições de aplicação à sua aplicação de função](functions-develop-vs.md#function-app-settings).

Quando se constrói o projeto, uma estrutura de pasta que se parece com o seguinte exemplo é gerada no diretório de saída de construção:

```
<framework.version>
 | - bin
 | - MyFirstFunction
 | | - function.json
 | - MySecondFunction
 | | - function.json
 | - host.json
```

Este diretório é o que é implementado na sua aplicação de função em Azure. As extensões de encadernação necessárias na [versão 2.x](functions-versions.md) do tempo de execução das Funções são [adicionadas ao projeto como pacotes NuGet](./functions-bindings-register.md#vs).

> [!IMPORTANT]
> O processo de construção cria uma *function.jsno* ficheiro para cada função. Esta *function.jsem* ficheiro não deve ser editada diretamente. Não é possível alterar a configuração de encadernação ou desativar a função editando este ficheiro. Para aprender a desativar uma função, consulte [Como desativar as funções](disable-function.md).


## <a name="methods-recognized-as-functions"></a>Métodos reconhecidos como funções

Numa biblioteca de classes, uma função é um método estático com um `FunctionName` atributo de gatilho e um gatilho, como mostra o seguinte exemplo:

```csharp
public static class SimpleExample
{
    [FunctionName("QueueTrigger")]
    public static void Run(
        [QueueTrigger("myqueue-items")] string myQueueItem, 
        ILogger log)
    {
        log.LogInformation($"C# function processed: {myQueueItem}");
    }
} 
```

O `FunctionName` atributo marca o método como ponto de entrada de função. O nome deve ser único dentro de um projeto, começar com uma letra e conter apenas letras, números `_` e `-` até 127 caracteres de comprimento. Os modelos de projeto muitas vezes criam um método chamado `Run` , mas o nome do método pode ser qualquer nome de método C# válido.

O atributo gatilho especifica o tipo de gatilho e liga os dados de entrada a um parâmetro do método. A função exemplo é desencadeada por uma mensagem de fila, e a mensagem de fila é passada para o método no `myQueueItem` parâmetro.

## <a name="method-signature-parameters"></a>Parâmetros de assinatura do método

A assinatura do método pode conter parâmetros diferentes dos utilizados com o atributo do gatilho. Aqui estão alguns dos outros parâmetros que pode incluir:

* [Entradas e encadernações de saída marcadas](functions-triggers-bindings.md) como tal, decorando-as com atributos.  
* Um `ILogger` `TraceWriter` parâmetro ou[(versão 1.x-only)](functions-versions.md#creating-1x-apps)para [o registo.](#logging)
* Um `CancellationToken` parâmetro para uma [paragem graciosa](#cancellation-tokens).
* [A ligação apresenta parâmetros](./functions-bindings-expressions-patterns.md) para obter metadados desencadeadores.

A ordem dos parâmetros na assinatura da função não importa. Por exemplo, pode colocar parâmetros de gatilho antes ou depois de outras encadernações, e pode colocar o parâmetro do madeireiros antes ou depois do gatilho ou parâmetros de ligação.

### <a name="output-bindings"></a>Enlaces de saída

Uma função pode ter ligações de saída zero ou uma definidas utilizando parâmetros de saída. 

O exemplo a seguir modifica o anterior adicionando uma fila de saída com o nome `myQueueItemCopy` . A função escreve o conteúdo da mensagem que desencadeia a função para uma nova mensagem numa fila diferente.

```csharp
public static class SimpleExampleWithOutput
{
    [FunctionName("CopyQueueMessage")]
    public static void Run(
        [QueueTrigger("myqueue-items-source")] string myQueueItem, 
        [Queue("myqueue-items-destination")] out string myQueueItemCopy,
        ILogger log)
    {
        log.LogInformation($"CopyQueueMessage function processed: {myQueueItem}");
        myQueueItemCopy = myQueueItem;
    }
}
```

Os valores atribuídos às ligações de saída são escritos quando a função sai. Pode utilizar mais do que uma ligação de saída numa função simplesmente atribuindo valores a vários parâmetros de saída. 

Os artigos de referência de encadernação[(filas de armazenamento,](functions-bindings-storage-queue.md)por exemplo) explicam quais os tipos de parâmetros que pode utilizar com atributos de detonação, entrada ou ligação de saída.

### <a name="binding-expressions-example"></a>Exemplo de expressões vinculativas

O código seguinte obtém o nome da fila para monitorizar a partir de uma configuração de aplicação, e obtém o tempo de criação de mensagem de fila no `insertionTime` parâmetro.

```csharp
public static class BindingExpressionsExample
{
    [FunctionName("LogQueueMessage")]
    public static void Run(
        [QueueTrigger("%queueappsetting%")] string myQueueItem,
        DateTimeOffset insertionTime,
        ILogger log)
    {
        log.LogInformation($"Message content: {myQueueItem}");
        log.LogInformation($"Created at: {insertionTime}");
    }
}
```

## <a name="autogenerated-functionjson"></a>function.jsautogerados em

O processo de construção cria uma *function.jsno* ficheiro numa pasta de função na pasta de construção. Como já foi notado anteriormente, este ficheiro não deve ser editado diretamente. Não é possível alterar a configuração de encadernação ou desativar a função editando este ficheiro. 

O objetivo deste ficheiro é fornecer informações ao controlador de escala para utilizar para [as decisões de escalonamento do plano de consumo.](event-driven-scaling.md) Por esta razão, o ficheiro tem apenas informações de gatilho, não encadernações de entrada/saída.

O *function.js* gerado no ficheiro inclui uma `configurationSource` propriedade que diz ao tempo de execução para usar atributos .NET para encadernações, em vez *defunction.jsna* configuração. Eis um exemplo:

```json
{
  "generatedBy": "Microsoft.NET.Sdk.Functions-1.0.0.0",
  "configurationSource": "attributes",
  "bindings": [
    {
      "type": "queueTrigger",
      "queueName": "%input-queue-name%",
      "name": "myQueueItem"
    }
  ],
  "disabled": false,
  "scriptFile": "..\\bin\\FunctionApp1.dll",
  "entryPoint": "FunctionApp1.QueueTrigger.Run"
}
```

## <a name="microsoftnetsdkfunctions"></a>Microsoft.NET.Sdk.Functions

A *function.jsna* geração de ficheiros é executada pelo pacote NuGet Microsoft NET [ \. \. Sdk \. Functions](https://www.nuget.org/packages/Microsoft.NET.Sdk.Functions). 

O mesmo pacote é utilizado tanto para as versões 1.x como para 2.x do tempo de execução das Funções. O quadro-alvo é o que diferencia um projeto de 1.x de um projeto 2.x. Aqui estão as partes relevantes dos *ficheiros .csproj,* mostrando diferentes quadros-alvo com o mesmo `Sdk` pacote:

# <a name="v2x"></a>[v2.x+](#tab/v2)

```xml
<PropertyGroup>
  <TargetFramework>netcoreapp2.1</TargetFramework>
  <AzureFunctionsVersion>v2</AzureFunctionsVersion>
</PropertyGroup>
<ItemGroup>
  <PackageReference Include="Microsoft.NET.Sdk.Functions" Version="1.0.8" />
</ItemGroup>
```

# <a name="v1x"></a>[v1.x](#tab/v1)

```xml
<PropertyGroup>
  <TargetFramework>net461</TargetFramework>
</PropertyGroup>
<ItemGroup>
  <PackageReference Include="Microsoft.NET.Sdk.Functions" Version="1.0.8" />
</ItemGroup>
```
---


Entre as dependências dos `Sdk` pacotes estão gatilhos e encadernações. Um projeto de 1.x refere-se a 1.x gatilhos e encadernações porque esses gatilhos e encadernações visam o Quadro .NET, enquanto 2.x dispara e liga o alvo .NET Core.

O `Sdk` pacote também depende de [Newtonsoft.Js,](https://www.nuget.org/packages/Newtonsoft.Json)e indiretamente no [WindowsAzure.Storage](https://www.nuget.org/packages/WindowsAzure.Storage). Estas dependências asseguram que o seu projeto utiliza as versões dos pacotes que funcionam com a versão de tempo de execução de Funções que o projeto visa. Por exemplo, `Newtonsoft.Json` tem a versão 11 para .NET Framework 4.6.1, mas o tempo de funcionamento das Funções que visa .NET Framework 4.6.1 só é compatível com `Newtonsoft.Json` 9.0.1. Assim, o seu código de função nesse projeto também tem de usar `Newtonsoft.Json` o 9.0.1.

O código-fonte `Microsoft.NET.Sdk.Functions` para está disponível nas funções gitHub repo [azure \- vs build \- \- \- sdk](https://github.com/Azure/azure-functions-vs-build-sdk).

## <a name="runtime-version"></a>Versão runtime

O Visual Studio utiliza as [Ferramentas Centrais Azure Functions](functions-run-local.md#install-the-azure-functions-core-tools) para executar projetos de funções. As Ferramentas Centrais são uma interface de linha de comando para o tempo de execução das funções.

Se instalar as Ferramentas Core utilizando npm, isso não afeta a versão Core Tools utilizada pelo Visual Studio. Para a versão runtime 1.x das Funções, o Visual Studio armazena as versões Core Tools em *%USERPROFILE%\AppData\Local\Azure.Functions.Cli* e utiliza a versão mais recente armazenada lá. Para as funções 2.x, as Ferramentas Core estão incluídas na extensão **Azure Functions e Web Jobs Tools.** Para 1.x e 2.x, pode ver que versão está a ser utilizada na saída da consola quando executar um projeto De funções:

```terminal
[3/1/2018 9:59:53 AM] Starting Host (HostId=contoso2-1518597420, Version=2.0.11353.0, ProcessId=22020, Debug=False, Attempt=0, FunctionsExtensionVersion=)
```

## <a name="readytorun"></a>ReadyToRun

Pode compilar a sua aplicação de função como [binários ReadyToRun](/dotnet/core/whats-new/dotnet-core-3-0#readytorun-images). ReadyToRun é uma forma de compilação antecipada que pode melhorar o desempenho do arranque para ajudar a reduzir o impacto do [arranque a frio](event-driven-scaling.md#cold-start) ao executar um plano de [consumo.](consumption-plan.md)

O ReadyToRun está disponível em .NET 3.0 e requer [a versão 3.0 do tempo de funcionamento das Funções Azure](functions-versions.md).

Para compilar o seu projeto como ReadyToRun, atualize o seu ficheiro de projeto adicionando os `<PublishReadyToRun>` elementos e `<RuntimeIdentifier>` elementos. Segue-se a configuração para publicação numa aplicação de função Windows 32 bit.

```xml
<PropertyGroup>
  <TargetFramework>netcoreapp3.1</TargetFramework>
  <AzureFunctionsVersion>v3</AzureFunctionsVersion>
  <PublishReadyToRun>true</PublishReadyToRun>
  <RuntimeIdentifier>win-x86</RuntimeIdentifier>
</PropertyGroup>
```

> [!IMPORTANT]
> O ReadyToRun não suporta atualmente a compilação cruzada. Tem de construir a sua aplicação na mesma plataforma que o alvo de implementação. Além disso, preste atenção à "mordida" que está configurada na sua aplicação de função. Por exemplo, se a sua aplicação de função no Azure for o Windows 64-bit, tem de compilar a sua aplicação no Windows `win-x64` com o identificador de [tempo de execução](/dotnet/core/rid-catalog).

Também pode construir a sua aplicação com ReadyToRun a partir da linha de comando. Para mais informações, consulte a `-p:PublishReadyToRun=true` opção em [`dotnet publish`](/dotnet/core/tools/dotnet-publish) .

## <a name="supported-types-for-bindings"></a>Tipos suportados para encadernações

Cada encadernação tem os seus próprios tipos suportados; por exemplo, um atributo de gatilho blob pode ser aplicado a um parâmetro de corda, um parâmetro POCO, um `CloudBlockBlob` parâmetro, ou qualquer um de vários outros tipos suportados. O [artigo de referência vinculativo para encadernações blob](functions-bindings-storage-blob-trigger.md#usage) lista todos os tipos de parâmetros suportados. Para obter mais informações, consulte [Gatilhos e encadernações](functions-triggers-bindings.md) e os [documentos de referência vinculativos para cada tipo de encadernação](functions-triggers-bindings.md#next-steps).

[!INCLUDE [HTTP client best practices](../../includes/functions-http-client-best-practices.md)]

## <a name="binding-to-method-return-value"></a>Ligação ao valor de retorno do método

Pode utilizar um valor de retorno de método para uma ligação de saída, aplicando o atributo ao valor de retorno do método. Por exemplo, consulte [Gatilhos e encadernações](./functions-bindings-return-value.md). 

Utilize o valor de devolução apenas se uma execução de função bem sucedida resultar sempre num valor de retorno para passar para a ligação de saída. Caso contrário, utilize `ICollector` `IAsyncCollector` ou, como indicado na secção seguinte.

## <a name="writing-multiple-output-values"></a>Escrever vários valores de saída

Para escrever vários valores para uma ligação de saída, ou se uma invocação de função bem sucedida pode não resultar em nada para passar para a ligação de saída, use os [`ICollector`](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/ICollector.cs) [`IAsyncCollector`](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/IAsyncCollector.cs) ou tipos. Estes tipos são coleções apenas de escrita que são escritas para a ligação de saída quando o método termina.

Este exemplo escreve várias mensagens de fila na mesma fila `ICollector` utilizando:

```csharp
public static class ICollectorExample
{
    [FunctionName("CopyQueueMessageICollector")]
    public static void Run(
        [QueueTrigger("myqueue-items-source-3")] string myQueueItem,
        [Queue("myqueue-items-destination")] ICollector<string> myDestinationQueue,
        ILogger log)
    {
        log.LogInformation($"C# function processed: {myQueueItem}");
        myDestinationQueue.Add($"Copy 1: {myQueueItem}");
        myDestinationQueue.Add($"Copy 2: {myQueueItem}");
    }
}
```

## <a name="async"></a>Async

Para fazer uma função [assíncrona,](/dotnet/csharp/programming-guide/concepts/async/)use a `async` palavra-chave e devolva um `Task` objeto.

```csharp
public static class AsyncExample
{
    [FunctionName("BlobCopy")]
    public static async Task RunAsync(
        [BlobTrigger("sample-images/{blobName}")] Stream blobInput,
        [Blob("sample-images-copies/{blobName}", FileAccess.Write)] Stream blobOutput,
        CancellationToken token,
        ILogger log)
    {
        log.LogInformation($"BlobCopy function processed.");
        await blobInput.CopyToAsync(blobOutput, 4096, token);
    }
}
```

Não pode usar `out` parâmetros em funções de async. Para encadernações de saída, utilize o valor de retorno da [função](#binding-to-method-return-value) ou um [objeto de coletor.](#writing-multiple-output-values)

## <a name="cancellation-tokens"></a>Fichas de cancelamento

Uma função pode aceitar um parâmetro [CancelamentoToken,](/dotnet/api/system.threading.cancellationtoken) que permite ao sistema operativo notificar o seu código quando a função está prestes a ser terminada. Pode utilizar esta notificação para se certificar de que a função não termina inesperadamente de forma a deixar os dados num estado inconsistente.

O exemplo a seguir mostra como verificar a interrupção da função iminente.

```csharp
public static class CancellationTokenExample
{
    public static void Run(
        [QueueTrigger("inputqueue")] string inputText,
        TextWriter logger,
        CancellationToken token)
    {
        for (int i = 0; i < 100; i++)
        {
            if (token.IsCancellationRequested)
            {
                logger.WriteLine("Function was cancelled at iteration {0}", i);
                break;
            }
            Thread.Sleep(5000);
            logger.WriteLine("Normal processing for queue message={0}", inputText);
        }
    }
}
```

## <a name="logging"></a>Registo

No seu código de função, pode escrever saídas para registos que aparecem como vestígios em Insights de Aplicação. A forma recomendada de escrever para os registos é incluir um parâmetro do tipo [ILogger,](/dotnet/api/microsoft.extensions.logging.ilogger)que é tipicamente nomeado `log` . Versão 1.x do tempo de execução de Funções utilizado `TraceWriter` , que também escreve para Application Insights, mas não suporta registos estruturados. Não use `Console.Write` para escrever os seus registos, uma vez que estes dados não são capturados pela Application Insights. 

### <a name="ilogger"></a>ILogger

Na definição de função, inclua um parâmetro [ILogger,](/dotnet/api/microsoft.extensions.logging.ilogger) que suporta [a exploração madeireira estruturada.](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging)

Com um `ILogger` objeto, você chama `Log<level>` [métodos de extensão no ILogger](/dotnet/api/microsoft.extensions.logging.loggerextensions#methods) para criar registos. O seguinte código escreve `Information` registos com `Function.<YOUR_FUNCTION_NAME>.User.` categoria:

```cs
public static async Task<HttpResponseMessage> Run(HttpRequestMessage req, ILogger logger)
{
    logger.LogInformation("Request for item with key={itemKey}.", id);
```

Para saber mais sobre como as funções `ILogger` implementam, consulte [recolher dados de telemetria](functions-monitoring.md#collecting-telemetry-data). Categorias pré-fixas com `Function` assumindo que está a usar um `ILogger` exemplo. Se optar por utilizar um `ILogger<T>` , o nome da categoria pode, em vez disso, basear-se em `T` .  

### <a name="structured-logging"></a>Registos estruturados

A ordem dos espaços reservados, não os seus nomes, determina quais os parâmetros utilizados na mensagem de registo. Suponha que tenha o seguinte código:

```csharp
string partitionKey = "partitionKey";
string rowKey = "rowKey";
logger.LogInformation("partitionKey={partitionKey}, rowKey={rowKey}", partitionKey, rowKey);
```

Se mantiver o mesmo fio de mensagem e inverter a ordem dos parâmetros, o texto de mensagem resultante terá os valores nos lugares errados.

Os espaços reservados são manuseados desta forma para que possa fazer a exploração madeireira estruturada. Application Insights armazena os pares de valor-nome do parâmetro e a cadeia de mensagem. O resultado é que os argumentos da mensagem tornam-se campos que pode consultar.

Se o seu método de madeirão se parecer com o exemplo anterior, pode consultar o campo `customDimensions.prop__rowKey` . O `prop__` prefixo é adicionado para garantir que não existem colisões entre os campos que o tempo de execução adiciona e os campos que o seu código de função adiciona.

Também pode consultar a cadeia de mensagem original fazendo referência ao campo `customDimensions.prop__{OriginalFormat}` .  

Aqui está uma amostra json representação de `customDimensions` dados:

```json
{
  "customDimensions": {
    "prop__{OriginalFormat}":"C# Queue trigger function processed: {message}",
    "Category":"Function",
    "LogLevel":"Information",
    "prop__message":"c9519cbf-b1e6-4b9b-bf24-cb7d10b1bb89"
  }
}
```

### <a name="log-custom-telemetry"></a><a name="log-custom-telemetry-in-c-functions"></a>Registar telemetria personalizada

Existe uma versão específica de Funções do SDK Application Insights que pode utilizar para enviar dados de telemetria personalizados das suas funções para Insights de Aplicação: [Microsoft.Azure.WebJobs.Logging.ApplicationInsights](https://www.nuget.org/packages/Microsoft.Azure.WebJobs.Logging.ApplicationInsights). Utilize o seguinte comando a partir da solicitação de comando para instalar este pacote:

# <a name="command"></a>[Comando](#tab/cmd)

```cmd
dotnet add package Microsoft.Azure.WebJobs.Logging.ApplicationInsights --version <VERSION>
```

# <a name="powershell"></a>[PowerShell](#tab/powershell)

```powershell
Install-Package Microsoft.Azure.WebJobs.Logging.ApplicationInsights -Version <VERSION>
```

---

Neste comando, `<VERSION>` substitua por uma versão deste pacote que suporte a sua versão instalada do [Microsoft.Azure.WebJobs](https://www.nuget.org/packages/Microsoft.Azure.WebJobs/). 

Os exemplos C# a seguir utilizam a [telemetria personalizada API](../azure-monitor/app/api-custom-events-metrics.md). O exemplo é para uma biblioteca de classe .NET, mas o código Application Insights é o mesmo para o script C#.

# <a name="v2x"></a>[v2.x+](#tab/v2)

As versões 2.x e posteriores do tempo de execução utilizam funcionalidades mais recentes no Application Insights para correlacionar automaticamente a telemetria com a operação atual. Não há necessidade de definir manualmente a `Id` `ParentId` operação, ou `Name` campos.

```cs
using System;
using System.Threading.Tasks;
using Microsoft.AspNetCore.Mvc;
using Microsoft.Azure.WebJobs;
using Microsoft.Azure.WebJobs.Extensions.Http;
using Microsoft.AspNetCore.Http;
using Microsoft.Extensions.Logging;

using Microsoft.ApplicationInsights;
using Microsoft.ApplicationInsights.DataContracts;
using Microsoft.ApplicationInsights.Extensibility;
using System.Linq;

namespace functionapp0915
{
    public class HttpTrigger2
    {
        private readonly TelemetryClient telemetryClient;

        /// Using dependency injection will guarantee that you use the same configuration for telemetry collected automatically and manually.
        public HttpTrigger2(TelemetryConfiguration telemetryConfiguration)
        {
            this.telemetryClient = new TelemetryClient(telemetryConfiguration);
        }

        [FunctionName("HttpTrigger2")]
        public Task<IActionResult> Run(
            [HttpTrigger(AuthorizationLevel.Anonymous, "get", Route = null)]
            HttpRequest req, ExecutionContext context, ILogger log)
        {
            log.LogInformation("C# HTTP trigger function processed a request.");
            DateTime start = DateTime.UtcNow;

            // Parse query parameter
            string name = req.Query
                .FirstOrDefault(q => string.Compare(q.Key, "name", true) == 0)
                .Value;

            // Write an event to the customEvents table.
            var evt = new EventTelemetry("Function called");
            evt.Context.User.Id = name;
            this.telemetryClient.TrackEvent(evt);

            // Generate a custom metric, in this case let's use ContentLength.
            this.telemetryClient.GetMetric("contentLength").TrackValue(req.ContentLength);

            // Log a custom dependency in the dependencies table.
            var dependency = new DependencyTelemetry
            {
                Name = "GET api/planets/1/",
                Target = "swapi.co",
                Data = "https://swapi.co/api/planets/1/",
                Timestamp = start,
                Duration = DateTime.UtcNow - start,
                Success = true
            };
            dependency.Context.User.Id = name;
            this.telemetryClient.TrackDependency(dependency);

            return Task.FromResult<IActionResult>(new OkResult());
        }
    }
}
```

Neste exemplo, os dados métricos personalizados são agregados pelo anfitrião antes de serem enviados para a tabela personalizadaMetrics. Para saber mais, consulte a documentação [GetMetric](../azure-monitor/app/api-custom-events-metrics.md#getmetric) em Application Insights. 

Ao correr localmente, deve adicionar a `APPINSIGHTS_INSTRUMENTATIONKEY` definição, com a tecla 'Insights de Aplicação', à [local.settings.jsficheiro.](functions-run-local.md#local-settings-file)


# <a name="v1x"></a>[v1.x](#tab/v1)

```cs
using System;
using System.Net;
using Microsoft.ApplicationInsights;
using Microsoft.ApplicationInsights.DataContracts;
using Microsoft.ApplicationInsights.Extensibility;
using Microsoft.Azure.WebJobs;
using System.Net.Http;
using System.Threading.Tasks;
using Microsoft.Azure.WebJobs.Extensions.Http;
using Microsoft.Extensions.Logging;
using System.Linq;

namespace functionapp0915
{
    public static class HttpTrigger2
    {
        private static string key = TelemetryConfiguration.Active.InstrumentationKey = 
            System.Environment.GetEnvironmentVariable(
                "APPINSIGHTS_INSTRUMENTATIONKEY", EnvironmentVariableTarget.Process);

        private static TelemetryClient telemetryClient = 
            new TelemetryClient() { InstrumentationKey = key };

        [FunctionName("HttpTrigger2")]
        public static async Task<HttpResponseMessage> Run(
            [HttpTrigger(AuthorizationLevel.Anonymous, "get", "post", Route = null)]
            HttpRequestMessage req, ExecutionContext context, ILogger log)
        {
            log.LogInformation("C# HTTP trigger function processed a request.");
            DateTime start = DateTime.UtcNow;

            // Parse query parameter
            string name = req.GetQueryNameValuePairs()
                .FirstOrDefault(q => string.Compare(q.Key, "name", true) == 0)
                .Value;

            // Get request body
            dynamic data = await req.Content.ReadAsAsync<object>();

            // Set name to query string or body data
            name = name ?? data?.name;
         
            // Track an Event
            var evt = new EventTelemetry("Function called");
            UpdateTelemetryContext(evt.Context, context, name);
            telemetryClient.TrackEvent(evt);
            
            // Track a Metric
            var metric = new MetricTelemetry("Test Metric", DateTime.Now.Millisecond);
            UpdateTelemetryContext(metric.Context, context, name);
            telemetryClient.TrackMetric(metric);
            
            // Track a Dependency
            var dependency = new DependencyTelemetry
            {
                Name = "GET api/planets/1/",
                Target = "swapi.co",
                Data = "https://swapi.co/api/planets/1/",
                Timestamp = start,
                Duration = DateTime.UtcNow - start,
                Success = true
            };
            UpdateTelemetryContext(dependency.Context, context, name);
            telemetryClient.TrackDependency(dependency);
        }
        
        // Correlate all telemetry with the current Function invocation
        private static void UpdateTelemetryContext(TelemetryContext context, ExecutionContext functionContext, string userName)
        {
            context.Operation.Id = functionContext.InvocationId.ToString();
            context.Operation.ParentId = functionContext.InvocationId.ToString();
            context.Operation.Name = functionContext.FunctionName;
            context.User.Id = userName;
        }
    }    
}
```
---

Não ligue `TrackRequest` ou `StartOperation<RequestTelemetry>` porque verá pedidos duplicados para uma invocação de função.  O tempo de execução das Funções rastreia automaticamente os pedidos.

Não se `telemetryClient.Context.Operation.Id` ateem. Esta definição global causa uma correlação incorreta quando muitas funções estão a funcionar simultaneamente. Em vez disso, crie uma nova instância de telemetria `DependencyTelemetry` (, `EventTelemetry` ) e modifique a sua `Context` propriedade. Em seguida, passe na instância da telemetria para o método correspondente `Track` em ( , , `TelemetryClient` `TrackDependency()` `TrackEvent()` `TrackMetric()` . Este método garante que a telemetria tem os detalhes corretos da correlação para a invocação da função atual.


## <a name="environment-variables"></a>Variáveis de ambiente

Para obter uma variável ambiental ou um valor de definição de aplicação, `System.Environment.GetEnvironmentVariable` utilize, como mostra o seguinte exemplo de código:

```csharp
public static class EnvironmentVariablesExample
{
    [FunctionName("GetEnvironmentVariables")]
    public static void Run([TimerTrigger("0 */5 * * * *")]TimerInfo myTimer, ILogger log)
    {
        log.LogInformation($"C# Timer trigger function executed at: {DateTime.Now}");
        log.LogInformation(GetEnvironmentVariable("AzureWebJobsStorage"));
        log.LogInformation(GetEnvironmentVariable("WEBSITE_SITE_NAME"));
    }

    private static string GetEnvironmentVariable(string name)
    {
        return name + ": " +
            System.Environment.GetEnvironmentVariable(name, EnvironmentVariableTarget.Process);
    }
}
```

As definições de aplicativos podem ser lidas a partir de variáveis ambientais, tanto quando se desenvolve localmente como quando está a correr em Azure. Ao desenvolver-se localmente, as configurações das aplicações provêm da `Values` coleção nolocal.settings.js *em* arquivo. Em ambos os ambientes, local e Azure, `GetEnvironmentVariable("<app setting name>")` recupera o valor da configuração da aplicação nomeada. Por exemplo, quando estiver a correr localmente, o "Meu Nome do Site" seria devolvido se o seu *local.settings.jsno* ficheiro contiver `{ "Values": { "WEBSITE_SITE_NAME": "My Site Name" } }` .

A [ propriedadeSystem.Configuration.ConfigurationManager.AppSettings](/dotnet/api/system.configuration.configurationmanager.appsettings) é uma API alternativa para obter valores de definição de aplicativos, mas recomendamos que use `GetEnvironmentVariable` como mostrado aqui.

## <a name="binding-at-runtime"></a>Encadernação no tempo de execução

Em C# e em outras línguas .NET, pode utilizar um padrão de ligação [imperativo,](https://en.wikipedia.org/wiki/Imperative_programming) em oposição às encadernações [*declarativas*](https://en.wikipedia.org/wiki/Declarative_programming) em atributos. A ligação imperativa é útil quando os parâmetros de ligação precisam de ser calculados em tempo de execução em vez de tempo de conceção. Com este padrão, pode ligar-se a entradas suportadas e ligações de saída no seu código de função.

Defina uma ligação imperativa da seguinte forma:

- **Não** inclua um atributo na assinatura de função para as suas ligações imperativas desejadas.
- Passe num parâmetro de entrada [`Binder binder`](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs.Host/Bindings/Runtime/Binder.cs) ou [`IBinder binder`](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/IBinder.cs) .
- Utilize o seguinte padrão C# para executar a ligação de dados.

  ```cs
  using (var output = await binder.BindAsync<T>(new BindingTypeAttribute(...)))
  {
      ...
  }
  ```

  `BindingTypeAttribute` é o atributo .NET que define a sua ligação, e `T` é um tipo de entrada ou saída que é suportado por esse tipo de ligação. `T` não pode ser um `out` tipo de parâmetro `out JObject` (como). Por exemplo, a ligação de saída da tabela mobile Apps suporta [seis tipos de saída](https://github.com/Azure/azure-webjobs-sdk-extensions/blob/master/src/WebJobs.Extensions.MobileApps/MobileTableAttribute.cs#L17-L22), mas só pode utilizar [iCollector \<T> ](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/ICollector.cs) ou [IAsyncCollector \<T> ](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/IAsyncCollector.cs) com obrigação de ligação.

### <a name="single-attribute-example"></a>Exemplo de atributo único

O seguinte código de exemplo cria uma [ligação de saída de bolha](functions-bindings-storage-blob-output.md) de armazenamento com o caminho blob definido no tempo de execução, em seguida, escreve uma corda para a bolha.

```cs
public static class IBinderExample
{
    [FunctionName("CreateBlobUsingBinder")]
    public static void Run(
        [QueueTrigger("myqueue-items-source-4")] string myQueueItem,
        IBinder binder,
        ILogger log)
    {
        log.LogInformation($"CreateBlobUsingBinder function processed: {myQueueItem}");
        using (var writer = binder.Bind<TextWriter>(new BlobAttribute(
                    $"samples-output/{myQueueItem}", FileAccess.Write)))
        {
            writer.Write("Hello World!");
        };
    }
}
```

[BlobAttribute](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs.Extensions.Storage/Blobs/BlobAttribute.cs) define a entrada [blob de armazenamento](functions-bindings-storage-blob.md) ou a ligação de saída, e [o TextWriter](/dotnet/api/system.io.textwriter) é um tipo de ligação de saída suportado.

### <a name="multiple-attributes-example"></a>Exemplo de vários atributos

O exemplo anterior obtém a definição da aplicação para a principal cadeia de ligação da conta de armazenamento da aplicação de função (que `AzureWebJobsStorage` é). Pode especificar uma definição de aplicação personalizada para utilizar para a conta de Armazenamento adicionando o [StorageAccountAttribute](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/StorageAccountAttribute.cs) e passando o conjunto de atributos em `BindAsync<T>()` . Use um `Binder` parâmetro, `IBinder` não.  Por exemplo:

```cs
public static class IBinderExampleMultipleAttributes
{
    [FunctionName("CreateBlobInDifferentStorageAccount")]
    public async static Task RunAsync(
            [QueueTrigger("myqueue-items-source-binder2")] string myQueueItem,
            Binder binder,
            ILogger log)
    {
        log.LogInformation($"CreateBlobInDifferentStorageAccount function processed: {myQueueItem}");
        var attributes = new Attribute[]
        {
        new BlobAttribute($"samples-output/{myQueueItem}", FileAccess.Write),
        new StorageAccountAttribute("MyStorageAccount")
        };
        using (var writer = await binder.BindAsync<TextWriter>(attributes))
        {
            await writer.WriteAsync("Hello World!!");
        }
    }
}
```

## <a name="triggers-and-bindings"></a>Acionadores e enlaces 

[!INCLUDE [Supported triggers and bindings](../../includes/functions-bindings.md)]

## <a name="next-steps"></a>Passos seguintes

> [!div class="nextstepaction"]
> [Saiba mais sobre gatilhos e encadernações](functions-triggers-bindings.md)

> [!div class="nextstepaction"]
> [Saiba mais sobre as melhores práticas para funções Azure](functions-best-practices.md)
