---
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 09/23/2019
ms.author: glenga
ms.openlocfilehash: 9b3859d854b3900cc30eda6a95b8425da6763e59
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/29/2021
ms.locfileid: "94341082"
---
Os atributos de ligação são definidos diretamente no function.jsno ficheiro. Dependendo do tipo de encadernação, podem ser necessárias propriedades adicionais. A [configuração de saída da fila](../articles/azure-functions/functions-bindings-storage-queue-output.md#configuration) descreve os campos necessários para uma encadernação de fila de armazenamento Azure. A extensão facilita a adição de encadernações à function.jsem ficheiro. 

Para criar uma ligação, clique à direita (Ctrl+clique no macOS) o `function.json` ficheiro na sua pasta HttpTrigger e escolha Adicionar **ligação...**. Siga as instruções para definir as seguintes propriedades vinculativas para a nova ligação:

| Prompt | Valor | Descrição |
| -------- | ----- | ----------- |
| **Selecione direção de encadernação** | `out` | A ligação é uma ligação de saída. |
| **Selecione a ligação com direção...** | `Azure Queue Storage` | A encadernação é uma encadernação de fila de armazenamento Azure. |
| **O nome usado para identificar esta ligação no seu código** | `msg` | Nome que identifica o parâmetro de ligação referenciado no seu código. |
| **A fila para a qual a mensagem será enviada** | `outqueue` | O nome da fila a que a ligação escreve. Quando o nome da *fila* não existe, a ligação cria-o na primeira utilização. |
| **Selecione a definição de "local.setting.json"** | `AzureWebJobsStorage` | O nome de uma definição de aplicação que contém o fio de ligação para a conta de Armazenamento. A `AzureWebJobsStorage` definição contém a cadeia de ligação para a conta de Armazenamento que criou com a aplicação de função. |

Uma ligação é adicionada à `bindings` matriz no seu function.js, que deve parecer o seguinte:

:::code language="json" source="~/functions-docs-javascript/functions-add-output-binding-storage-queue-cli/HttpExample/function.json" range="18-24":::
