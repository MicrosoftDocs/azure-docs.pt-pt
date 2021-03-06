---
title: 'Quickstart: Biblioteca de clientes de gestão Azure para .NET'
description: Neste quickstart, começa com a biblioteca de clientes de gestão Azure para .NET.
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.topic: include
ms.date: 3/22/2021
ms.author: pafarley
ms.openlocfilehash: 1ecfd58e961e70182bfc2e0cc8eaf77f5053eb77
ms.sourcegitcommit: 42e4f986ccd4090581a059969b74c461b70bcac0
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/23/2021
ms.locfileid: "104879698"
---
[Documentação de referência](/dotnet/api/overview/azure/cognitiveservices/management)  |  [Código fonte da biblioteca](https://github.com/Azure/azure-sdk-for-net/tree/master/sdk/cognitiveservices/Microsoft.Azure.Management.CognitiveServices)  |  [Pacote (NuGet)](https://www.nuget.org/packages/Microsoft.Azure.Management.CognitiveServices/)  |  [Amostras](https://github.com/Azure/azure-sdk-for-net/tree/master/sdk/cognitiveservices/Microsoft.Azure.Management.CognitiveServices/tests)

## <a name="c-prerequisites"></a>C# pré-requisitos

* Uma subscrição válida da Azure - [Crie uma gratuitamente.](https://azure.microsoft.com/free/)
* A versão atual de [.NET Core](https://dotnet.microsoft.com/download/dotnet-core).

[!INCLUDE [Create a service principal](./create-service-principal.md)]

[!INCLUDE [Create a resource group](./create-resource-group.md)]

## <a name="create-a-new-c-application"></a>Criar uma nova aplicação C#

Criar uma nova aplicação .NET Core. Numa janela de consola (como cmd, PowerShell ou Bash), utilize o `dotnet new` comando para criar uma nova aplicação de consola com o nome `azure-management-quickstart` . Este comando cria um projeto "Hello World" C# simples com um único ficheiro de origem: *programa.cs*. 

```console
dotnet new console -n azure-management-quickstart
```

Mude o seu diretório para a pasta de aplicações recém-criada. Pode construir a aplicação com:

```console
dotnet build
```

A saída de construção não deve conter avisos ou erros. 

```console
...
Build succeeded.
 0 Warning(s)
 0 Error(s)
...
```

### <a name="install-the-client-library"></a>Instalar a biblioteca do cliente

No diretório de aplicações, instale a biblioteca de clientes da Azure Management para .NET com o seguinte comando:

```console
dotnet add package Microsoft.Azure.Management.CognitiveServices
dotnet add package Microsoft.Azure.Management.Fluent
dotnet add package Microsoft.Azure.Management.ResourceManager.Fluent
```

Se estiver a utilizar o Visual Studio IDE, a biblioteca do cliente está disponível como um pacote NuGet transferível.

### <a name="import-libraries"></a>Importar bibliotecas

Abra *o programa.cs* e adicione as `using` seguintes declarações ao topo do ficheiro:

[!code-csharp[](~/cognitive-services-quickstart-code/dotnet/azure_management_service/create_delete_resource.cs?name=snippet_using)]

## <a name="authenticate-the-client"></a>Autenticar o cliente

Adicione os seguintes campos à raiz do *programa.cs* e povoe os seus valores, utilizando o principal serviço que criou e as informações da sua conta Azure.

[!code-csharp[](~/cognitive-services-quickstart-code/dotnet/azure_management_service/create_delete_resource.cs?name=snippet_constants)]

Em seguida, no seu método **principal,** use estes valores para construir um objeto **CognitiveServicesManagementClient.** Este objeto é necessário para todas as suas operações de gestão Azure.

[!code-csharp[](~/cognitive-services-quickstart-code/dotnet/azure_management_service/create_delete_resource.cs?name=snippet_assigns)]

## <a name="call-management-methods"></a>Métodos de gestão de chamadas

Adicione o seguinte código ao seu método **Principal** para listar os recursos disponíveis, criar um recurso de amostra, listar os seus recursos próprios e, em seguida, eliminar o recurso da amostra. Definirá estes métodos nos próximos passos.

[!code-csharp[](~/cognitive-services-quickstart-code/dotnet/azure_management_service/create_delete_resource.cs?name=snippet_calls)]

## <a name="create-a-cognitive-services-resource-c"></a>Criar um recurso de Serviços Cognitivos (C#)

Para criar e subscrever um novo recurso de Serviços Cognitivos, utilize o método **Criar.** Este método adiciona um novo recurso faturado ao grupo de recursos em que passa. Ao criar o seu novo recurso, terá de conhecer o "tipo" de serviço que pretende utilizar, juntamente com o seu nível de preços (ou SKU) e uma localização Azure. O seguinte método toma todos estes como argumentos e cria um recurso.

[!code-csharp[](~/cognitive-services-quickstart-code/dotnet/azure_management_service/create_delete_resource.cs?name=snippet_create)]

### <a name="choose-a-service-and-pricing-tier"></a>Escolha um nível de serviço e preços

Quando criar um novo recurso, terá de conhecer o "tipo" de serviço que pretende utilizar, juntamente com o [nível de preços](https://azure.microsoft.com/pricing/details/cognitive-services/) (ou SKU) que pretende. Usará esta e outras informações como parâmetros ao criar o recurso. Pode encontrar uma lista de "tipos" disponíveis do Serviço Cognitivo, chamando o seguinte método no seu script:

[!code-csharp[](~/cognitive-services-quickstart-code/dotnet/azure_management_service/create_delete_resource.cs?name=snippet_list_avail)]

[!INCLUDE [cognitive-services-subscription-types](../../../../includes/cognitive-services-subscription-types.md)]

[!INCLUDE [SKUs and pricing](./sku-pricing.md)]

## <a name="view-your-resources"></a>Ver os seus recursos

Para visualizar todos os recursos na sua conta Azure (em todos os grupos de recursos), utilize o seguinte método:

[!code-csharp[](~/cognitive-services-quickstart-code/dotnet/azure_management_service/create_delete_resource.cs?name=snippet_list)]

## <a name="delete-a-resource"></a>Eliminar um recurso

O seguinte método elimina o recurso especificado do grupo de recursos dado.

[!code-csharp[](~/cognitive-services-quickstart-code/dotnet/azure_management_service/create_delete_resource.cs?name=snippet_delete)]

## <a name="run-the-application"></a>Executar a aplicação

Executar o pedido do seu diretório de candidaturas com o `dotnet run` comando.

```dotnet
dotnet run
```

## <a name="see-also"></a>Ver também

* Consulte **[pedidos autenticados aos Serviços Cognitivos da Azure](../../authentication.md)** sobre como trabalhar de forma segura com os Serviços Cognitivos.
* Veja **[o que são os Serviços Cognitivos Azure para](../../what-are-cognitive-services.md)** obter uma lista de diferentes categorias dentro dos Serviços Cognitivos.
* Consulte o **[suporte linguístico natural](../../language-support.md)** para ver a lista de línguas naturais que os Serviços Cognitivos suportam.
* Consulte **[o Uso de Serviços Cognitivos como recipientes](../../cognitive-services-container-support.md)** para entender como utilizar os Serviços Cognitivos no pré-pré-m.
* Consulte **[o Plano e gere os custos dos Serviços Cognitivos](../../plan-manage-costs.md)** para estimar o custo da utilização dos Serviços Cognitivos.
* Consulte **[a documentação de referência da Cognitive Services Management SDK](/dotnet/api/overview/azure/cognitiveservices/management)** para obter mais detalhes sobre a gestão SDK.
