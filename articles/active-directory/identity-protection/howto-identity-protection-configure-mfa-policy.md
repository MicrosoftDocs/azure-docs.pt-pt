---
title: Configure a política de registo do MFA - Azure Ative Directory Identity Protection
description: Saiba como configurar a política de registo de autenticação multi-factor Azure AD Protection.
services: active-directory
ms.service: active-directory
ms.subservice: identity-protection
ms.topic: how-to
ms.date: 06/05/2020
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: sahandle
ms.collection: M365-identity-device-management
ms.openlocfilehash: 072db1d47abd95844075aeedfeddc4f8cf6bf936
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/29/2021
ms.locfileid: "94835871"
---
# <a name="how-to-configure-the-azure-ad-multi-factor-authentication-registration-policy"></a>Como: Configurar a política de registo de autenticação multi-factor Azure AD

A Azure AD Identity Protection ajuda-o a gerir o lançamento do registo de Autenticação Multi-Factor (MFA) Azure AD, configurando uma política de Acesso Condicional para exigir o registo de MFA independentemente da aplicação de autenticação moderna a que esteja a iniciar sessão.

## <a name="what-is-the-azure-ad-multi-factor-authentication-registration-policy"></a>O que é a política de registo de autenticação multi-factor Azure AD?

A Azure AD Multi-Factor Authentication fornece um meio para verificar quem está a usar mais do que apenas um nome de utilizador e senha. Fornece uma segunda camada de segurança para os insus de sômposições do utilizador. Para que os utilizadores possam responder às solicitações de MFA, devem primeiro registar-se para autenticação multi-factor Azure AD.

Recomendamos que exija autenticação multi-factor Azure AD para os inserções do utilizador porque:

- Proporciona autenticação forte através de uma gama de opções de verificação.
- Desempenha um papel fundamental na preparação da sua organização para se auto-remediar das deteções de risco na Proteção de Identidade.

Para obter mais informações sobre autenticação multi-factor Azure AD, consulte [o que é a autenticação multi-factor Azure AD?](../authentication/howto-mfa-getstarted.md)

## <a name="policy-configuration"></a>Configuração da política

1. Navegue até ao [portal Azure.](https://portal.azure.com)
1. Consulte a política de registo  >    >  de MFA de **proteção de identidade**  >  de segurança ativa do Azure Ative.
   1. Em **Atribuições**
      1. **Utilizadores** - Escolha **todos os utilizadores** ou **selecione indivíduos e grupos** se limitar o seu lançamento.
         1. Opcionalmente pode optar por excluir os utilizadores da apólice.
   1. Sob **controlo**
      1. Certifique-se de que a caixa **de verificação Requer registo Azure AD MFA** é verificada e escolha **Selecione**.
   1. **Fazer cumprir a política**  -  **Em**
   1. **Guardar**

## <a name="user-experience"></a>Experiência do utilizador

A Azure Ative Directory Identity Protection vai levar os seus utilizadores a registarem-se na próxima vez que assinarem interativamente e terão 14 dias para completar o registo. Durante este período de 14 dias, podem contornar a inscrição, mas no final do período serão obrigados a registar-se antes de poderem concluir o processo de inscrição.

Para uma visão geral da experiência do utilizador relacionado, consulte:

- [Experiências de inscrição com a Azure AD Identity Protection](concept-identity-protection-user-experience.md).  

## <a name="next-steps"></a>Passos seguintes

- [Ativar políticas de insusição e risco de utilizador](howto-identity-protection-configure-risk-policies.md)

- [Ativar o reset da palavra-passe de autosserviço AZure AD](../authentication/howto-sspr-deployment.md)

- [Ativar o Multi-Factor Authentication do Azure AD](../authentication/howto-mfa-getstarted.md)
