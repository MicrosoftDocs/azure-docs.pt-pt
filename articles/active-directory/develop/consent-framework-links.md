---
title: Como funciona o consentimento da aplicação
description: Saiba mais sobre como funciona o quadro de consentimento Azure AD para ver como pode usá-lo ao desenvolver aplicações em Azure AD
services: active-directory
author: rwike77
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.custom: aaddev
ms.workload: identity
ms.topic: conceptual
ms.date: 09/11/2018
ms.author: ryanwi
ROBOTS: NOINDEX
ms.openlocfilehash: 9fa910dee2830f6749f0fbd36f065c31dafa6757
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/29/2021
ms.locfileid: "101646253"
---
# <a name="how-application-consent-works"></a>Como funciona o consentimento da aplicação

Este artigo é para ajudá-lo a aprender mais sobre como funciona o quadro de consentimento Azure AD para que possa desenvolver aplicações de forma mais eficaz.

## <a name="recommended-documents"></a>Documentos recomendados

- Obtenha uma compreensão geral de [como o consentimento permite que um proprietário de recurso regule o acesso de uma aplicação aos recursos.](./developer-glossary.md#consent)
- Obtenha uma visão geral passo a passo de como o quadro de [consentimento Azure AD implementa o consentimento](./quickstart-register-app.md).
- Para obter mais profundidade, saiba [como uma aplicação multi-inquilino pode usar o quadro de consentimento](./howto-convert-app-to-be-multi-tenant.md) para implementar o consentimento "utilizador" e "administrador", suportando padrões de aplicação de vários níveis mais avançados.
- Para obter mais profundidade, saiba [como o consentimento é suportado na camada do protocolo OAuth 2.0 durante o fluxo de concessão de código de autorização.](../azuread-dev/v1-protocols-oauth-code.md#request-an-authorization-code)

## <a name="next-steps"></a>Passos seguintes
[AzureAD Microsoft Q&A](/answers/topics/azure-active-directory.html)