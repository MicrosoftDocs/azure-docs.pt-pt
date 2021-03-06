---
title: Métodos e funcionalidades de autenticação - Azure Ative Directory
description: Conheça os diferentes métodos e funcionalidades de autenticação disponíveis no Azure Ative Directory para ajudar a melhorar e garantir eventos de entrada
services: active-directory
ms.service: active-directory
ms.subservice: authentication
ms.topic: conceptual
ms.date: 03/15/2021
ms.author: justinha
author: justinha
manager: daveba
ms.collection: M365-identity-device-management
ms.custom: contperf-fy20q4
ms.openlocfilehash: b4d69157f4544daad962cca15e53802e7b912399
ms.sourcegitcommit: 49b2069d9bcee4ee7dd77b9f1791588fe2a23937
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/16/2021
ms.locfileid: "107530435"
---
# <a name="what-authentication-and-verification-methods-are-available-in-azure-active-directory"></a>Que métodos de autenticação e verificação estão disponíveis no Azure Active Directory?

Como parte da experiência de inscrição para contas no Azure Ative Directory (Azure AD), existem diferentes formas de um utilizador se autenticar. Um nome de utilizador e palavra-passe é a forma mais comum de um utilizador fornecer credenciais historicamente. Com as funcionalidades modernas de autenticação e segurança no Azure AD, essa palavra-passe básica deve ser completada ou substituída por métodos de autenticação mais seguros.

![Tabela dos pontos fortes e dos métodos de autenticação preferidos em Azure AD](media/concept-authentication-methods/authentication-methods.png)

Métodos de autenticação sem palavras-passe, tais como o Windows Hello, as teclas de segurança FIDO2 e a aplicação Microsoft Authenticator fornecem os eventos de entrada mais seguros.

A Azure AD Multi-Factor Authentication (MFA) adiciona segurança adicional apenas usando uma palavra-passe quando um utilizador assina. O utilizador pode ser solicitado para formas adicionais de autenticação, tais como responder a uma notificação push, introduzir um código a partir de um sinal de software ou hardware, ou responder a uma SMS ou chamada telefónica.

Para simplificar a experiência de embarque do utilizador e registar-se tanto para o MFA como para o autosserviço de senha reset (SSPR), recomendamos que [você permita o registo combinado de informações de segurança](howto-registration-mfa-sspr-combined.md). Para a resiliência, recomendamos que exija que os utilizadores registem vários métodos de autenticação. Quando um método não está disponível para um utilizador durante a sing-in ou SSPR, pode optar por autenticar com outro método. Para obter mais informações, consulte [Criar uma estratégia resiliente de gestão de controlo de acessos em Azure AD.](concept-resilient-controls.md)

Aqui está um [vídeo](https://www.youtube.com/watch?v=LB2yj4HSptc&feature=youtu.be) que criamos para ajudá-lo a escolher o melhor método de autenticação para manter a sua organização segura.

## <a name="authentication-method-strength-and-security"></a>Força e segurança do método de autenticação

Quando implementar funcionalidades como Azure AD Multi-Factor Authentication na sua organização, reveja os métodos de autenticação disponíveis. Escolha os métodos que satisfaçam ou excedam os seus requisitos em termos de segurança, usabilidade e disponibilidade. Sempre que possível, utilize métodos de autenticação com o mais alto nível de segurança.

O quadro que se segue descreve as considerações de segurança para os métodos de autenticação disponíveis. A disponibilidade é uma indicação de que o utilizador pode utilizar o método de autenticação, não da disponibilidade de serviço em Azure AD:

| Método de autenticação          | Segurança | Usabilidade | Disponibilidade |
|--------------------------------|:--------:|:---------:|:------------:|
| Windows Hello para empresas     | Alto     | Alto      | Alto         |
| Aplicação Microsoft Authenticator    | Alto     | Alto      | Alto         |
| Chave de segurança FIDO2             | Alto     | Alto      | Alto         |
| Fichas de hardware do OATH (pré-visualização) | Médio   | Médio    | Alto         |
| Fichas de software OATH           | Médio   | Médio    | Alto         |
| SMS                            | Médio   | Alto      | Médio       |
| Voz                          | Médio   | Médio    | Médio       |
| Palavra-passe                       | Baixo      | Alto      | Alto         |

Para obter as últimas informações sobre segurança, confira as nossas publicações de blog:

- [É hora de desligar nos transportes telefónicos para a autenticação](https://techcommunity.microsoft.com/t5/azure-active-directory-identity/it-s-time-to-hang-up-on-phone-transports-for-authentication/ba-p/1751752)
- [Vulnerabilidades de autenticação e vetores de ataque](https://techcommunity.microsoft.com/t5/azure-active-directory-identity/all-your-creds-are-belong-to-us/ba-p/855124)

> [!TIP]
> Para flexibilidade e usabilidade, recomendamos que utilize a aplicação Microsoft Authenticator. Este método de autenticação proporciona a melhor experiência do utilizador e vários modos, tais como notificações push sem palavras-passe, notificações push MFA e códigos OATH.

## <a name="how-each-authentication-method-works"></a>Como funciona cada método de autenticação

Alguns métodos de autenticação podem ser usados como fator principal quando se inscreve numa aplicação ou dispositivo, como a utilização de uma chave de segurança FIDO2 ou uma palavra-passe. Outros métodos de autenticação só estão disponíveis como fator secundário quando utiliza a Autenticação Multi-Factor AD Azure ou SSPR.

Os seguintes contornos da tabela quando um método de autenticação pode ser utilizado durante um evento de entrada:

| Método                         | Autenticação primária | Autenticação secundária  |
|--------------------------------|:----------------------:|:-------------------------:|
| Windows Hello para empresas     | Yes                    | MFA                       |
| Aplicação Microsoft Authenticator    | Yes                    | MFA e SSPR              |
| Chave de segurança FIDO2             | Yes                    | MFA                       |
| Fichas de hardware do OATH (pré-visualização) | No                     | MFA                       |
| Fichas de software OATH           | No                     | MFA                       |
| SMS                            | Yes                    | MFA e SSPR              |
| Chamada de voz                     | No                     | MFA e SSPR              |
| Palavra-passe                       | Yes                    |                           |

Todos estes métodos de autenticação podem ser configurados no portal Azure e, cada vez mais, utilizando a API do [Microsoft Graph REST](/graph/api/resources/authenticationmethods-overview).

Para saber mais sobre o funcionamento de cada método de autenticação, consulte os seguintes artigos conceptuais separados:

* [Windows Hello para empresas](/windows/security/identity-protection/hello-for-business/hello-overview)
* [Aplicação Microsoft Authenticator](concept-authentication-authenticator-app.md)
* [Chave de segurança FIDO2](concept-authentication-passwordless.md#fido2-security-keys)
* [Fichas de hardware do OATH (pré-visualização)](concept-authentication-oath-tokens.md#oath-hardware-tokens-preview)
* [Fichas de software OATH](concept-authentication-oath-tokens.md#oath-software-tokens)
* [Sindução](howto-authentication-sms-signin.md) e [verificação](concept-authentication-phone-options.md#mobile-phone-verification) por SMS
* [Verificação de chamadas de voz](concept-authentication-phone-options.md)
* Palavra-passe

> [!NOTE]
> Em Azure AD, uma palavra-passe é frequentemente um dos métodos de autenticação primária. Não é possível desativar o método de autenticação de palavras-passe. Se utilizar uma palavra-passe como fator de autenticação primária, aumente a segurança dos eventos de entrada através da autenticação multi-factor Azure AD.

Em certos cenários, podem ser utilizados os seguintes métodos de verificação adicionais:

* [Passwords de aplicação](howto-mfa-app-passwords.md) - usadas para aplicações antigas que não suportam a autenticação moderna e podem ser configuradas para a autenticação multi-factor Azure AD por utilizador.
* [Questões de segurança](concept-authentication-security-questions.md) - apenas utilizadas para sSPR
* [Endereço de e-mail](concept-sspr-howitworks.md#authentication-methods) - usado apenas para SSPR

## <a name="next-steps"></a>Passos seguintes

Para começar, consulte o [tutorial para reset de senha de autosserviço (SSPR)][tutorial-sspr] e [autenticação multi-factor AD Azure][tutorial-azure-mfa].

Para saber mais sobre os conceitos SSPR, consulte [como funciona o reset da palavra-passe de autosserviço AZure AD][concept-sspr].

Para saber mais sobre os conceitos de MFA, consulte [como funciona a autenticação multi-factor Azure AD.][concept-mfa]

Saiba mais sobre a configuração dos métodos de autenticação utilizando a API do [Microsoft Graph REST](/graph/api/resources/authenticationmethods-overview).

Para rever os métodos de autenticação em uso, consulte a análise do [método de autenticação multi-factor Azure AD com o PowerShell](/samples/azure-samples/azure-mfa-authentication-method-analysis/azure-mfa-authentication-method-analysis/).

<!-- INTERNAL LINKS -->
[tutorial-sspr]: tutorial-enable-sspr.md
[tutorial-azure-mfa]: tutorial-enable-azure-mfa.md
[concept-sspr]: concept-sspr-howitworks.md
[concept-mfa]: concept-mfa-howitworks.md
