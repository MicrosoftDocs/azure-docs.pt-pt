---
title: 'Tutorial: Trabalhar com filas de armazenamento de fila Azure em .NET'
description: Um tutorial sobre a utilização do Azure Queue Storage para criar filas e inserir, obter e apagar mensagens usando o código .NET.
author: twooley
ms.author: twooley
ms.reviewer: dineshm
ms.date: 06/09/2020
ms.topic: tutorial
ms.service: storage
ms.subservice: queues
ms.custom: devx-track-csharp
ms.openlocfilehash: dda6fb816fcba4ab9350edd956d895a3b9b204d4
ms.sourcegitcommit: 02bc06155692213ef031f049f5dcf4c418e9f509
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/03/2021
ms.locfileid: "106276132"
---
# <a name="tutorial-work-with-azure-queue-storage-queues-in-net"></a>Tutorial: Trabalhar com filas de armazenamento de fila Azure em .NET

O Azure Queue Storage implementa filas baseadas em nuvem para permitir a comunicação entre componentes de uma aplicação distribuída. Cada fila mantém uma lista de mensagens que podem ser adicionadas por um componente remetente e processadas por um componente recetor. Com uma fila, a sua aplicação pode escalar imediatamente para satisfazer a procura. Este artigo mostra os passos básicos para trabalhar com uma fila de armazenamento de fila Azure.

Neste tutorial, ficará a saber como:

> [!div class="checklist"]
>
> - Criar uma conta de Armazenamento do Azure
> - Criar a aplicação
> - Adicione as bibliotecas de clientes Azure
> - Adicionar suporte para código assíncronos
> - Criar uma fila
> - Insira as mensagens numa fila
> - Dequeue mensagens
> - Apagar uma fila vazia
> - Verifique se há argumentos de linha de comando
> - Compilar e executar a aplicação

## <a name="prerequisites"></a>Pré-requisitos

- Obtenha a sua cópia gratuita do editor do Código do Estúdio Visual da [plataforma.](https://code.visualstudio.com/download)
- Faça o download e instale a versão 3.1 ou posterior [.NET Core SDK.](https://dotnet.microsoft.com/download)
- Se não tiver uma subscrição atual do Azure, crie uma [conta gratuita](https://azure.microsoft.com/free/) antes de começar.

## <a name="create-an-azure-storage-account"></a>Criar uma conta de Armazenamento do Azure

Primeiro, crie uma conta de Armazenamento Azure. Para obter um guia passo a passo para a criação de uma conta de armazenamento, consulte [Criar uma conta de armazenamento](../common/storage-account-create.md?toc=%2Fazure%2Fstorage%2Fqueues%2Ftoc.json). Este é um passo separado que executa depois de criar uma conta Azure gratuita nos pré-requisitos.

## <a name="create-the-app"></a>Criar a aplicação

Criar uma aplicação .NET Core denominada `QueueApp` . Para simplificar, esta aplicação irá enviar e receber mensagens através da fila.

1. Numa janela de consola (como cmd, PowerShell ou Azure CLI), utilize o `dotnet new` comando para criar uma nova aplicação de consola com o nome `QueueApp` . Este comando cria um projeto C# simples "olá mundo" com um único ficheiro de origem denominado `Program.cs` .

   ```console
   dotnet new console -n QueueApp
   ```

2. Mude para pasta `QueueApp` acabada de criar e compile a aplicação, para confirmar que está tudo bem.

   ```console
   cd QueueApp
   ```

   ```console
   dotnet build
   ```

   Deve ver resultados semelhantes aos seguintes:

   ```output
   C:\Tutorials>dotnet new console -n QueueApp
   The template "Console Application" was created successfully.

   Processing post-creation actions...
   Running 'dotnet restore' on QueueApp\QueueApp.csproj...
     Restore completed in 155.63 ms for C:\Tutorials\QueueApp\QueueApp.csproj.

   Restore succeeded.

   C:\Tutorials>cd QueueApp

   C:\Tutorials\QueueApp>dotnet build
   Microsoft (R) Build Engine version 16.0.450+ga8dc7f1d34 for .NET Core
   Copyright (C) Microsoft Corporation. All rights reserved.

     Restore completed in 40.87 ms for C:\Tutorials\QueueApp\QueueApp.csproj.
     QueueApp -> C:\Tutorials\QueueApp\bin\Debug\netcoreapp3.1\QueueApp.dll

   Build succeeded.
       0 Warning(s)
       0 Error(s)

   Time Elapsed 00:00:02.40

   C:\Tutorials\QueueApp>_
   ```

<!-- markdownlint-disable MD023 -->

## <a name="add-the-azure-client-libraries"></a>Adicione as bibliotecas de clientes Azure

1. Adicione as bibliotecas de clientes Azure Storage ao projeto utilizando o `dotnet add package` comando.

   # <a name="net-v12"></a>[.NET v12](#tab/dotnet)

   Executar o seguinte comando a partir da pasta do projeto na janela da consola.

   ```console
   dotnet add package Azure.Storage.Queues
   ```

   # <a name="net-v11"></a>[.NET v11](#tab/dotnetv11)

   Executar os seguintes comandos a partir da pasta do projeto na janela da consola.

   ```console
   dotnet add package Microsoft.Azure.Storage.Common
   ```

   ```console
   dotnet add package Microsoft.Azure.Storage.Queue
   ```
   ---

### <a name="add-using-statements"></a>Adicionar usando declarações

1. A partir da linha de comando no diretório do projeto, `code .` escreva para abrir o Código do Estúdio Visual no atual diretório. Mantenha a janela da linha de comando aberta. Haverá mais ordens para executar mais tarde. Se lhe for solicitado que adicione ativos C# necessários para construir e depurar, clique no botão **Sim.**

1. Abra o `Program.cs` ficheiro de origem e adicione os seguintes espaços de nome logo após a `using System;` declaração. Esta aplicação utiliza tipos destes espaços de nome para ligar ao Azure Storage e trabalhar com filas.

   # <a name="net-v12"></a>[.NET v12](#tab/dotnet)

   :::code language="csharp" source="~/azure-storage-snippets/queues/tutorial/dotnet/dotnet-v12/QueueApp/Program.cs" id="snippet_UsingStatements":::

   # <a name="net-v11"></a>[.NET v11](#tab/dotnetv11)

   :::code language="csharp" source="~/azure-storage-snippets/queues/tutorial/dotnet/dotnet-v11/QueueApp/Program.cs" id="snippet_UsingStatements":::

1. Guarde o ficheiro `Program.cs`.

## <a name="add-support-for-asynchronous-code"></a>Adicionar suporte para código assíncronos

Uma vez que a aplicação utiliza recursos em nuvem, o código é executado de forma assíncronea.

1. Atualize o `Main` método para executar assíncronos. `void`Substitua-o por um `async Task` valor de retorno.

   ```csharp
   static async Task Main(string[] args)
   ```

1. Guarde o ficheiro `Program.cs`.

## <a name="create-a-queue"></a>Criar uma fila

Antes de fazer chamadas para a AZure APIs, deve obter as suas credenciais do portal Azure.

[!INCLUDE [storage-quickstart-credentials-include](../../../includes/storage-quickstart-credentials-include.md)]

### <a name="add-the-connection-string-to-the-app"></a>Adicione o fio de ligação à aplicação

Adicione o fio de ligação na aplicação para que possa aceder à conta de armazenamento.

1. Mude de volta para Visual Studio Code.

1. No `Main` método, substitua o `Console.WriteLine("Hello, World");` código pela seguinte linha que obtém a cadeia de ligação da variável ambiente.

   # <a name="net-v12"></a>[.NET v12](#tab/dotnet)

   :::code language="csharp" source="~/azure-storage-snippets/queues/tutorial/dotnet/dotnet-v12/QueueApp/Program.cs" id="snippet_DeclareConnectionString":::

   # <a name="net-v11"></a>[.NET v11](#tab/dotnetv11)

   :::code language="csharp" source="~/azure-storage-snippets/queues/tutorial/dotnet/dotnet-v11/QueueApp/Program.cs" id="snippet_DeclareConnectionString":::

1. Adicione o seguinte código `Main` para criar um objeto de fila, que é posteriormente passado para os métodos de envio e receção.

   # <a name="net-v12"></a>[.NET v12](#tab/dotnet)

   :::code language="csharp" source="~/azure-storage-snippets/queues/tutorial/dotnet/dotnet-v12/QueueApp/Program.cs" id="snippet_CreateQueueClient":::

   # <a name="net-v11"></a>[.NET v11](#tab/dotnetv11)

   :::code language="csharp" source="~/azure-storage-snippets/queues/tutorial/dotnet/dotnet-v11/QueueApp/Program.cs" id="snippet_CreateQueueClient":::

1. Guarde o ficheiro.

## <a name="insert-messages-into-the-queue"></a>Insira mensagens na fila

Crie um novo método para enviar uma mensagem para a fila.

1. Adicione o seguinte `InsertMessageAsync` método à sua `Program` classe.

   # <a name="net-v12"></a>[.NET v12](#tab/dotnet)

   Este método é aprovado como referência de fila. Uma nova fila é criada, se já não existe, [`CreateIfNotExistsAsync`](/dotnet/api/azure.storage.queues.queueclient.createifnotexistsasync) chamando. Em seguida, adiciona a `newMessage` fila chamando [`SendMessageAsync`](/dotnet/api/azure.storage.queues.queueclient.sendmessageasync) .

   :::code language="csharp" source="~/azure-storage-snippets/queues/tutorial/dotnet/dotnet-v12/QueueApp/Program.cs" id="snippet_InsertMessage":::

   # <a name="net-v11"></a>[.NET v11](#tab/dotnetv11)

   Este método é aprovado como referência de fila. Uma nova fila é criada, se já não existe, [`CreateIfNotExistsAsync`](/dotnet/api/microsoft.azure.storage.queue.cloudqueue.createifnotexistsasync) chamando. Em seguida, adiciona a `newMessage` fila chamando [`AddMessageAsync`](/dotnet/api/microsoft.azure.storage.queue.cloudqueue.addmessageasync) .

   :::code language="csharp" source="~/azure-storage-snippets/queues/tutorial/dotnet/dotnet-v11/QueueApp/Program.cs" id="snippet_InsertMessage":::

1. **Opcional:** Por predefinição, o tempo máximo de vida para uma mensagem está definido para sete dias. Pode especificar qualquer número positivo para o tempo de vida da mensagem. O seguinte corte de código adiciona uma mensagem que **nunca** expira.

   # <a name="net-v12"></a>[.NET v12](#tab/dotnet)

    Para adicionar uma mensagem que não expire, use `Timespan.FromSeconds(-1)` na sua chamada para `SendMessageAsync` .

   :::code language="csharp" source="~/azure-storage-snippets/queues/tutorial/dotnet/dotnet-v12/QueueApp/Initial.cs" id="snippet_SendNonExpiringMessage":::

   # <a name="net-v11"></a>[.NET v11](#tab/dotnetv11)

    Para adicionar uma mensagem que não expire, use `Timespan.FromSeconds(-1)` na sua chamada para `AddMessageAsync` .

   :::code language="csharp" source="~/azure-storage-snippets/queues/tutorial/dotnet/dotnet-v11/QueueApp/Initial.cs" id="snippet_SendNonExpiringMessage":::

1. Guarde o ficheiro.

Uma mensagem de fila deve ser num formato compatível com um pedido de XML utilizando a codificação UTF-8. Uma mensagem pode ter até 64 KB de tamanho. Se uma mensagem contiver dados binários, codificar a mensagem [base64.](/dotnet/api/system.convert.tobase64string)

## <a name="dequeue-messages"></a>Dequeue mensagens

Crie um novo método para recuperar uma mensagem da fila. Uma vez que a mensagem é recebida com sucesso, é importante apagá-la da fila para que não seja processada mais de uma vez.

1. Adicione um novo método chamado `RetrieveNextMessageAsync` à sua `Program` classe.

   # <a name="net-v12"></a>[.NET v12](#tab/dotnet)

   Este método recebe uma mensagem da fila [`ReceiveMessagesAsync`](/dotnet/api/azure.storage.queues.queueclient.receivemessagesasync) chamando, passando `1` no primeiro parâmetro para recuperar apenas a próxima mensagem na fila. Depois de receber a mensagem, elimine-a da fila ligando [`DeleteMessageAsync`](/dotnet/api/azure.storage.queues.queueclient.deletemessageasync) .

   Quando uma mensagem é enviada para a fila com uma versão do SDK antes do v12, é automaticamente codificada base64. Começando pelo v12, esta funcionalidade foi removida. Quando recupera uma mensagem utilizando o V12 SDK, esta não é automaticamente descodificada na Base64. Deve explicitamente [a Base64 descodificar](/dotnet/api/system.convert.frombase64string) o conteúdo por si mesmo.

   :::code language="csharp" source="~/azure-storage-snippets/queues/tutorial/dotnet/dotnet-v12/QueueApp/Initial.cs" id="snippet_InitialRetrieveMessage":::

   # <a name="net-v11"></a>[.NET v11](#tab/dotnetv11)

   Este método recebe uma mensagem da fila ligando [`GetMessageAsync`](/dotnet/api/microsoft.azure.storage.queue.cloudqueue.getmessageasync) . Depois de receber a mensagem, elimine-a da fila ligando [`DeleteMessageAsync`](/dotnet/api/microsoft.azure.storage.queue.cloudqueue.deletemessageasync) .

   :::code language="csharp" source="~/azure-storage-snippets/queues/tutorial/dotnet/dotnet-v11/QueueApp/Initial.cs" id="snippet_InitialRetrieveMessage":::

1. Guarde o ficheiro.

## <a name="delete-an-empty-queue"></a>Apagar uma fila vazia

É uma boa prática no final de um projeto para identificar se ainda precisa dos recursos que criou. Os recursos que deixar em execução podem custar-lhe dinheiro. Se a fila existe mas está vazia, pergunte ao utilizador se pretende eliminá-la.

1. Expanda o `RetrieveNextMessageAsync` método para incluir um pedido para eliminar a fila vazia.

   # <a name="net-v12"></a>[.NET v12](#tab/dotnet)

   :::code language="csharp" source="~/azure-storage-snippets/queues/tutorial/dotnet/dotnet-v12/QueueApp/Program.cs" id="snippet_RetrieveMessage":::

   # <a name="net-v11"></a>[.NET v11](#tab/dotnetv11)

   :::code language="csharp" source="~/azure-storage-snippets/queues/tutorial/dotnet/dotnet-v11/QueueApp/Program.cs" id="snippet_RetrieveMessage":::

1. Guarde o ficheiro.

## <a name="check-for-command-line-arguments"></a>Verifique se há argumentos de linha de comando

Se houver argumentos de linha de comando passados na app, assuma que são uma mensagem a ser adicionada à fila. Junte os argumentos juntos para fazer uma corda. Adicione este fio à fila da mensagem chamando o `InsertMessageAsync` método que adicionamos anteriormente.

Se não houver argumentos de linha de comando, tente uma operação de recuperação. Ligue para o `RetrieveNextMessageAsync` método para recuperar a próxima mensagem na fila.

Por fim, aguarde a entrada do utilizador antes de sair, ligando `Console.ReadLine` .

1. Expanda o `Main` método para verificar se há argumentos de linha de comando e aguarde a entrada do utilizador.

   # <a name="net-v12"></a>[.NET v12](#tab/dotnet)

   :::code language="csharp" source="~/azure-storage-snippets/queues/tutorial/dotnet/dotnet-v12/QueueApp/Program.cs" id="snippet_Main":::

   # <a name="net-v11"></a>[.NET v11](#tab/dotnetv11)

   :::code language="csharp" source="~/azure-storage-snippets/queues/tutorial/dotnet/dotnet-v11/QueueApp/Program.cs" id="snippet_Main":::

1. Guarde o ficheiro.

## <a name="complete-code"></a>Código completo

Aqui está a lista completa de códigos para este projeto.

   # <a name="net-v12"></a>[.NET v12](#tab/dotnet)

   :::code language="csharp" source="~/azure-storage-snippets/queues/tutorial/dotnet/dotnet-v12/QueueApp/Program.cs" id="snippet_AllCode":::

   # <a name="net-v11"></a>[.NET v11](#tab/dotnetv11)

   :::code language="csharp" source="~/azure-storage-snippets/queues/tutorial/dotnet/dotnet-v11/QueueApp/Program.cs" id="snippet_AllCode":::
   ---

## <a name="build-and-run-the-app"></a>Compilar e executar a aplicação

1. A partir da linha de comando no diretório do projeto, executar o seguinte comando dotnet para construir o projeto.

   ```console
   dotnet build
   ```

1. Depois de o projeto construir com sucesso, executar o seguinte comando para adicionar a primeira mensagem à fila.

   ```console
   dotnet run First queue message
   ```

   Deverá ver este resultado:

   ```output
   C:\Tutorials\QueueApp>dotnet run First queue message
   The queue was created.
   Sent: First queue message
   Press Enter..._
   ```

1. Executar a aplicação sem argumentos de linha de comando para receber e remover a primeira mensagem na fila.

   ```console
   dotnet run
   ```

1. Continue a executar a aplicação até que todas as mensagens sejam removidas. Se o executar mais uma vez, receberá uma mensagem de que a fila está vazia e um pedido para apagar a fila.

   ```output
   C:\Tutorials\QueueApp>dotnet run First queue message
   The queue was created.
   Sent: First queue message
   Press Enter...

   C:\Tutorials\QueueApp>dotnet run Second queue message
   Sent: Second queue message
   Press Enter...

   C:\Tutorials\QueueApp>dotnet run Third queue message
   Sent: Third queue message
   Press Enter...

   C:\Tutorials\QueueApp>dotnet run
   Received: First queue message
   Press Enter...

   C:\Tutorials\QueueApp>dotnet run
   Received: Second queue message
   Press Enter...

   C:\Tutorials\QueueApp>dotnet run
   Received: Third queue message
   Press Enter...

   C:\Tutorials\QueueApp>dotnet run
   The queue is empty. Attempt to delete it? (Y/N) Y
   Received: The queue was deleted.
   Press Enter...

   C:\Tutorials\QueueApp>_
   ```

## <a name="next-steps"></a>Passos seguintes

Neste tutorial, ficou a saber como:

1. Criar uma fila
1. Adicione e remova mensagens de uma fila
1. Apagar uma fila de armazenamento de fila Azure

Consulte os quickstarts de armazenamento da fila Azure para obter mais informações.

> [!div class="nextstepaction"]
> [Filas rápidas para o portal](storage-quickstart-queues-portal.md)

- [Filas de arranque rápido para .NET](storage-quickstart-queues-dotnet.md)
- [Filas rápidas para Java](storage-quickstart-queues-java.md)
- [Filas rápidas para Python](storage-quickstart-queues-python.md)
- [Filas rápidas para JavaScript](storage-quickstart-queues-nodejs.md)
