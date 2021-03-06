---
title: Construa uma aplicação de uma página única chamando uma API web
titleSuffix: Microsoft identity platform
description: Saiba como construir uma aplicação de uma página que chame uma API web
services: active-directory
author: rwike77
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: conceptual
ms.workload: identity
ms.date: 05/06/2019
ms.author: ryanwi
ms.custom: aaddev
ms.openlocfilehash: 4dd4a40233fc1c030581fd1ae2827061435a0ab3
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/29/2021
ms.locfileid: "98753537"
---
# <a name="single-page-application-call-a-web-api"></a>Aplicação de uma página: Ligue para uma API web

Recomendamos que ligue para o `acquireTokenSilent` método para adquirir ou renovar um token de acesso antes de chamar uma API web. Depois de ter um token, pode chamar uma API web protegida.

## <a name="call-a-web-api"></a>Chamar uma API Web

# <a name="javascript"></a>[JavaScript](#tab/javascript)

Utilize o token de acesso adquirido como portador num pedido HTTP para ligar para qualquer API web, como a Microsoft Graph API. Por exemplo:

```javascript
    var headers = new Headers();
    var bearer = "Bearer " + access_token;
    headers.append("Authorization", bearer);
    var options = {
         method: "GET",
         headers: headers
    };
    var graphEndpoint = "https://graph.microsoft.com/v1.0/me";

    fetch(graphEndpoint, options)
        .then(function (response) {
             //do something with response
        }
```

# <a name="angular"></a>[Angular](#tab/angular)

O invólucro ANGULAR MSAL aproveita o intercetor HTTP para adquirir automaticamente os tokens de acesso silenciosamente e anexá-los aos pedidos HTTP às APIs. Para mais informações, consulte [Adquirir um token para chamar uma API.](scenario-spa-acquire-token.md)

---

## <a name="next-steps"></a>Passos seguintes

Passe para o próximo artigo neste cenário, [passe para a produção](scenario-spa-production.md).