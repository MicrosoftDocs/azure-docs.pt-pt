---
title: incluir ficheiro
description: incluir ficheiro
services: event-grid
author: spelluru
ms.service: event-grid
ms.topic: include
ms.date: 08/17/2018
ms.author: spelluru
ms.custom: include file
ms.openlocfilehash: cd778da5fd53cb8744a9f267384fcc8e11941582
ms.sourcegitcommit: 32e0fedb80b5a5ed0d2336cea18c3ec3b5015ca1
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/30/2021
ms.locfileid: "105645389"
---
## <a name="enable-the-event-grid-resource-provider"></a>Ativar o fornecedor de recursos da Grelha de Eventos

Se ainda não utilizou a Grade de Eventos na sua subscrição Azure, poderá ter de registar o fornecedor de recursos da Grelha de Eventos. Execute o seguinte comando para registar o fornecedor:

```azurecli-interactive
az provider register --namespace Microsoft.EventGrid
```

Pode levar um momento para o registo terminar. Para ver o estado, execute:

```azurecli-interactive
az provider show --namespace Microsoft.EventGrid --query "registrationState"
```

Quando `registrationState` está `Registered`, está pronto para continuar.
