---
title: incluir ficheiro
description: incluir ficheiro
services: azure-communication-services
author: mikben
manager: mikben
ms.service: azure-communication-services
ms.subservice: azure-communication-services
ms.date: 03/10/2021
ms.topic: include
ms.custom: include file
ms.author: mikben
ms.openlocfilehash: 4c8bd66dde54ff90ea2191fba58f10c87c45cf68
ms.sourcegitcommit: 32e0fedb80b5a5ed0d2336cea18c3ec3b5015ca1
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/30/2021
ms.locfileid: "105958332"
---
## <a name="prerequisites"></a>Pré-requisitos
Antes de começar, certifique-se de:
- Crie uma conta Azure com uma subscrição ativa. Para mais detalhes, consulte [Criar uma conta gratuitamente.](https://azure.microsoft.com/free/?WT.mc_id=A261C142F)
- Instalar [Estúdio Visual](https://visualstudio.microsoft.com/downloads/)
- Criar um recurso Azure Communication Services. Para mais detalhes, consulte [Criar um Recurso de Comunicação Azure](../../create-communication-resource.md). Terá de gravar o seu **ponto final** de recurso para este arranque rápido.
- Um [token de acesso ao utilizador](../../access-tokens.md). Certifique-se de definir o âmbito para "chat", e note a cadeia simbólica, bem como a cadeia userId.

## <a name="setting-up"></a>Configuração

### <a name="create-a-new-c-application"></a>Criar uma nova aplicação C#

Numa janela de consola (como cmd, PowerShell ou Bash), utilize o `dotnet new` comando para criar uma nova aplicação de consola com o nome `ChatQuickstart` . Este comando cria um projeto "Hello World" C# com um único ficheiro de origem: **Programa.cs**.

```console
dotnet new console -o ChatQuickstart
```

Mude o seu diretório para a pasta de aplicação recém-criada e use o `dotnet build` comando para compilar a sua aplicação.

```console
cd ChatQuickstart
dotnet build
```

### <a name="install-the-package"></a>Instale o pacote

Instale o Azure Communication Chat SDK para .NET

```PowerShell
dotnet add package Azure.Communication.Chat --version 1.0.0
```

## <a name="object-model"></a>Modelo de objeto

As seguintes aulas lidam com algumas das principais características do Azure Communication Services Chat SDK para C#.

| Nome                                  | Descrição                                                  |
| ------------------------------------- | ------------------------------------------------------------ |
| ChatClient | Esta classe é necessária para a funcionalidade Chat. Você instantaneamente com as suas informações de subscrição, e usá-lo para criar, obter e apagar fios. |
| ChatThreadClient | Esta classe é necessária para a funcionalidade Chat Thread. Obtém um caso através do ChatClient e utiliza-o para enviar/receber/atualizar/apagar mensagens, adicionar/remover/receber participantes, enviar notificações de dactilografia e ler recibos. |

## <a name="create-a-chat-client"></a>Criar um cliente de chat

Para criar um cliente de chat, utilizará o seu ponto final dos Serviços de Comunicação e o token de acesso que foi gerado como parte dos passos pré-requisitos. Você precisa usar a `CommunicationIdentityClient` classe a partir do SDK de identidade para criar um utilizador e emitir um token para passar para o seu cliente de chat.

Saiba mais sobre [tokens de acesso ao utilizador.](../../access-tokens.md)

Este quickstart não cobre a criação de um nível de serviço para gerir fichas para a sua aplicação de chat, embora seja recomendado. Saiba mais sobre [a Arquitetura chat](../../../concepts/chat/concepts.md)

Copie os seguintes fragmentos de código e cole-os no ficheiro de origem: **Programa.cs**
```csharp
using Azure;
using Azure.Communication;
using Azure.Communication.Chat;
using System;

namespace ChatQuickstart
{
    class Program
    {
        static async System.Threading.Tasks.Task Main(string[] args)
        {
            // Your unique Azure Communication service endpoint
            Uri endpoint = new Uri("https://<RESOURCE_NAME>.communication.azure.com");

            CommunicationTokenCredential communicationTokenCredential = new CommunicationTokenCredential(<Access_Token>);
            ChatClient chatClient = new ChatClient(endpoint, communicationTokenCredential);
        }
    }
}
```

## <a name="start-a-chat-thread"></a>Inicie um fio de chat

Use o `createChatThread` método no chatClient para criar um fio de chat
- Use `topic` para dar um tópico a este chat; O tópico pode ser atualizado após a criação do fio de chat utilizando a `UpdateTopic` função.
- Utilize `participants` propriedade para passar uma lista de `ChatParticipant` objetos a adicionar ao fio de chat. O `ChatParticipant` objeto é inicializado com um `CommunicationIdentifier` objeto. `CommunicationIdentifier` pode ser do `CommunicationUserIdentifier` tipo, `MicrosoftTeamsUserIdentifier` ou `PhoneNumberIdentifier` . Por exemplo, para obter um `CommunicationIdentifier` objeto, terá de passar um ID de acesso que criou seguindo instruções para [criar um utilizador](../../access-tokens.md#create-an-identity)

O objeto de resposta do `createChatThread` método contém os `chatThread` detalhes. Para interagir com as operações do fio de chat, como adicionar participantes, enviar uma mensagem, apagar uma mensagem, etc., uma `chatThreadClient` instância do cliente precisa de ser instantânea usando o método no `GetChatThreadClient` `ChatClient` cliente.

```csharp
var chatParticipant = new ChatParticipant(identifier: new CommunicationUserIdentifier(id: "<Access_ID>"))
{
    DisplayName = "UserDisplayName"
};
CreateChatThreadResult createChatThreadResult = await chatClient.CreateChatThreadAsync(topic: "Hello world!", participants: new[] { chatParticipant });
ChatThreadClient chatThreadClient = chatClient.GetChatThreadClient(threadId: createChatThreadResult.ChatThread.Id);
string threadId = chatThreadClient.Id;
```

## <a name="get-a-chat-thread-client"></a>Obtenha um cliente de linha de chat
O `GetChatThreadClient` método devolve um cliente de linha para um fio que já existe. Pode ser utilizado para a realização de operações no fio criado: adicionar membros, enviar mensagem, etc. threadId é o ID único do fio de chat existente.

```csharp
string threadId = "<THREAD_ID>";
ChatThreadClient chatThreadClient = chatClient.GetChatThreadClient(threadId: threadId);
```

## <a name="list-all-chat-threads"></a>Listar todos os fios de chat
Utilize `GetChatThreads` para recuperar todos os fios de chat dos que o utilizador faz parte.

```csharp
AsyncPageable<ChatThreadItem> chatThreadItems = chatClient.GetChatThreadsAsync();
await foreach (ChatThreadItem chatThreadItem in chatThreadItems)
{
    Console.WriteLine($"{ chatThreadItem.Id}");
}
```

## <a name="send-a-message-to-a-chat-thread"></a>Envie uma mensagem para um fio de chat

Use `SendMessage` para enviar uma mensagem para um fio.

- Utilização `content` para fornecer o conteúdo da mensagem, é necessário.
- Utilize `type` para o tipo de conteúdo da mensagem, como 'Texto' ou 'Html'. Se não for especificado, será definido 'Texto'.
- Utilize `senderDisplayName` para especificar o nome de visualização do remetente. Se não for especificado, a corda vazia será definida.

```csharp
SendChatMessageResult sendChatMessageResult = await chatThreadClient.SendMessageAsync(content:"hello world", type: ChatMessageType.Text);
string messageId = sendChatMessageResult.Id;
```

## <a name="receive-chat-messages-from-a-chat-thread"></a>Receba mensagens de chat de um fio de chat

Pode recuperar mensagens de chat sondando o `GetMessages` método no cliente do fio de chat em intervalos especificados.

```csharp
AsyncPageable<ChatMessage> allMessages = chatThreadClient.GetMessagesAsync();
await foreach (ChatMessage message in allMessages)
{
    Console.WriteLine($"{message.Id}:{message.Content.Message}");
}
```

`GetMessages` toma um `DateTimeOffset` parâmetro opcional. Se essa compensação for especificada, receberá mensagens que foram recebidas, atualizadas ou eliminadas após a sua aprovação. Note que as mensagens recebidas antes do tempo de compensação, mas editadas ou removidas depois de serem devolvidas também serão devolvidas.

`GetMessages` retorna a versão mais recente da mensagem, incluindo quaisquer edições ou eliminações que aconteceram com a mensagem usando `UpdateMessage` e `DeleteMessage` . Para mensagens eliminadas, `chatMessage.DeletedOn` de retorna um valor de hora de data indicando quando essa mensagem foi eliminada. Para mensagens editadas, `chatMessage.EditedOn` retorna uma data indicando quando a mensagem foi editada. A hora original da criação de mensagens pode ser acedida através da `chatMessage.CreatedOn` utilização , podendo ser utilizada para encomendar as mensagens.

`GetMessages` retorna diferentes tipos de mensagens que podem ser identificadas por `chatMessage.Type` . Estes tipos são:

- `Text`: Mensagem de chat regular enviada por um membro do thread.

- `Html`: Uma mensagem de texto formatada. Note que os utilizadores dos Serviços de Comunicação não podem enviar mensagens RichText. Este tipo de mensagem é suportado por mensagens enviadas de utilizadores de Equipas para utilizadores de Serviços de Comunicação em cenários de Teams Interop.

- `TopicUpdated`: Mensagem do sistema que indica que o tópico foi atualizado. (só)

- `ParticipantAdded`: Mensagem do sistema que indica que um ou mais participantes foram adicionados ao fio de conversação. (só)

- `ParticipantRemoved`: Mensagem do sistema que indica que um participante foi removido do fio de conversação.

Para mais detalhes, consulte [os Tipos de Mensagens](../../../concepts/chat/concepts.md#message-types).

## <a name="add-a-user-as-a-participant-to-the-chat-thread"></a>Adicione um utilizador como participante ao fio de chat

Uma vez criado um fio, pode adicionar e remover os utilizadores do mesmo. Ao adicionar os utilizadores, dá-lhes acesso para poderem enviar mensagens para o fio e adicionar/remover outros participantes. Antes de `AddParticipants` ligar, certifique-se de que adquiriu um novo token de acesso e identidade para esse utilizador. O utilizador necessitará desse token de acesso para inicializar o seu cliente de chat.

Utilize `AddParticipants` para adicionar um ou mais participantes ao fio de chat. Seguem-se os atributos suportados para cada participante do fio:
- `communicationUser`, necessário, é a identidade do participante do fio.
- `displayName`, opcional, é o nome de exibição do participante do thread.
- `shareHistoryTime`, opcional, tempo a partir do qual a história do chat é partilhada com o participante.

```csharp
var josh = new CommunicationUserIdentifier(id: "<Access_ID_For_Josh>");
var gloria = new CommunicationUserIdentifier(id: "<Access_ID_For_Gloria>");
var amy = new CommunicationUserIdentifier(id: "<Access_ID_For_Amy>");

var participants = new[]
{
    new ChatParticipant(josh) { DisplayName = "Josh" },
    new ChatParticipant(gloria) { DisplayName = "Gloria" },
    new ChatParticipant(amy) { DisplayName = "Amy" }
};

await chatThreadClient.AddParticipantsAsync(participants: participants);
```

## <a name="get-thread-participants"></a>Receba participantes de thread

Utilize `GetParticipants` para recuperar os participantes do fio de chat.

```csharp
AsyncPageable<ChatParticipant> allParticipants = chatThreadClient.GetParticipantsAsync();
await foreach (ChatParticipant participant in allParticipants)
{
    Console.WriteLine($"{((CommunicationUserIdentifier)participant.User).Id}:{participant.DisplayName}:{participant.ShareHistoryTime}");
}
```

## <a name="send-read-receipt"></a>Enviar recibo de leitura

Utilize `SendReadReceipt` para notificar outros participantes de que a mensagem é lida pelo utilizador.

```csharp
await chatThreadClient.SendReadReceiptAsync(messageId: messageId);
```

## <a name="run-the-code"></a>Executar o código

Executar o pedido do seu diretório de candidaturas com o `dotnet run` comando.

```console
dotnet run
```
