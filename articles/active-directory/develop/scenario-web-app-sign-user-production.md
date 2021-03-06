---
title: Mover aplicação web que assina nos utilizadores para produção | Rio Azure
titleSuffix: Microsoft identity platform
description: Saiba como construir uma aplicação web que assina nos utilizadores (passe para a produção)
services: active-directory
author: jmprieur
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: conceptual
ms.workload: identity
ms.date: 09/17/2019
ms.author: jmprieur
ms.custom: aaddev
ms.openlocfilehash: f670af1fca4b4638988e53075f092ca1bbac55b2
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/30/2021
ms.locfileid: "104578265"
---
# <a name="web-app-that-signs-in-users-move-to-production"></a>Aplicação web que assina nos utilizadores: Mover-se para a produção

Agora que sabe como obter um símbolo para chamar APIs web, aqui ficam algumas coisas a considerar ao mover a sua aplicação para a produção.

[!INCLUDE [Common steps to move to production](../../../includes/active-directory-develop-scenarios-production.md)]

## <a name="troubleshooting"></a>Resolução de problemas
Quando os utilizadores iniciarem a s presenciação na aplicação web pela primeira vez, terão de consentir. No entanto, em algumas organizações, os utilizadores podem ver uma mensagem como a seguinte: *O AppName precisa de permissões para aceder a recursos na sua organização que só um administrador pode conceder. Por favor, peça a um administrador que conceda permissão a esta aplicação antes de poder usá-la.*
Isto porque o administrador do seu inquilino **desativou** a capacidade de os utilizadores consentirem. Nesse caso, contacte os administradores do seu inquilino para que façam um consentimento administrativo para os âmbitos exigidos pelo pedido.

## <a name="same-site"></a>Mesmo site

Certifique-se de que compreende possíveis problemas com novas versões do navegador Chrome: [Como lidar com as alterações de cookies SameSite no navegador Chrome](howto-handle-samesite-cookie-changes-chrome-browser.md).

O pacote Microsoft.Identity.Web NuGet lida com os problemas SameSite mais comuns.

## <a name="deep-dive-aspnet-core-web-app-tutorial"></a>Mergulho profundo: tutorial de aplicação web core ASP.NET Core

Saiba mais sobre outras formas de assinar nos utilizadores com este tutorial ASP.NET Core: 

[Ative as suas aplicações web para iniciar sípis nos utilizadores e ligue para APIs com a plataforma de identidade da Microsoft para programadores](https://github.com/Azure-Samples/ms-identity-aspnetcore-webapp-tutorial)

Este tutorial progressivo tem código pronto para a produção para uma aplicação web, incluindo como adicionar o início de sôr-in com contas em:

- A sua organização
- Múltiplas organizações
- Contas de trabalho ou escola, ou contas pessoais da Microsoft
- [Azure AD B2C](../../active-directory-b2c/overview.md)
- Clouds nacionais

## <a name="tutorial-nodejs-web-app"></a>Tutorial: Node.js web app

Saiba mais sobre a web Node.js neste tutorial:

[Tutorial: Utilizadores de inscrição numa aplicação web Node.js & Express](https://docs.microsoft.com/azure/active-directory/develop/tutorial-v2-nodejs-webapp-msal)

## <a name="sample-code-java-web-app"></a>Código de amostra: Java web app

Saiba mais sobre a aplicação web Java a partir desta amostra no GitHub: 

[Uma aplicação Java Web que assina nos utilizadores com a plataforma de identidade da Microsoft e chama o Microsoft Graph](https://github.com/Azure-Samples/ms-identity-java-webapp)

## <a name="next-steps"></a>Passos Seguintes

Depois de a sua aplicação web assinar nos utilizadores, pode ligar para apis web em nome dos utilizadores inscritos. Chamar APIs web da aplicação web é o objeto do seguinte cenário: [aplicação web que chama APIs web](scenario-web-app-call-api-overview.md).