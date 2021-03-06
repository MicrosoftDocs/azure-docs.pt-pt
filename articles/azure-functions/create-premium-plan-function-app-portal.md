---
title: Criar um plano Azure Functions Premium no portal
description: Saiba como usar o portal Azure para criar uma aplicação de função que funciona no plano Premium.
ms.topic: how-to
ms.date: 10/30/2020
ms.openlocfilehash: 9cab67f096665c9333fa40bcb790896fcbebd8d5
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/29/2021
ms.locfileid: "98676591"
---
# <a name="create-a-premium-plan-function-app-in-the-azure-portal"></a>Criar uma aplicação de função de plano Premium no portal Azure

A Azure Functions oferece um plano Premium escalável que proporciona conectividade de rede virtual, sem arranque a frio e hardware premium. Para saber mais, consulte [o plano Azure Functions Premium.](functions-premium-plan.md) 

Neste artigo, aprende-se a usar o portal Azure para criar uma aplicação de função num plano Premium. 

## <a name="sign-in-to-azure"></a>Iniciar sessão no Azure

Inicie sessão no [portal do Azure](https://portal.azure.com) com a sua conta do Azure.

## <a name="create-a-function-app"></a>Criar uma aplicação de funções

Precisa de uma aplicação Function App para alojar a execução das suas funções. Uma aplicação de função permite-lhe agrupar funções como uma unidade lógica para facilitar a gestão, implementação, escala e partilha de recursos.

[!INCLUDE [functions-premium-create](../../includes/functions-premium-create.md)]

Neste momento, pode criar funções na nova aplicação de funções. Estas funções podem tirar partido dos benefícios do [plano Premium.](functions-premium-plan.md)

## <a name="clean-up-resources"></a>Limpar os recursos

[!INCLUDE [Clean-up resources](../../includes/functions-quickstart-cleanup.md)]

## <a name="next-steps"></a>Passos seguintes

> [!div class="nextstepaction"]
> [Adicione uma função detonada HTTP] (./functions-get-started.md
